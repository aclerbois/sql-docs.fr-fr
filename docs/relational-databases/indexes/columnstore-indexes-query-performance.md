---
title: Index columnstore - Performances des requêtes | Microsoft Docs
ms.custom: ''
ms.date: 12/01/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.suite: sql
ms.technology: table-view-index
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 83acbcc4-c51e-439e-ac48-6d4048eba189
caps.latest.revision: 23
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: c1cb86de5b9fa7ef0f6efed087e9f308567c8927
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="columnstore-indexes---query-performance"></a>Index columnstore - Performances des requêtes
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

  Les index columnstore sont conçus pour rendre le traitement des requêtes beaucoup plus rapide. Les recommandations suivantes vous permettront d’atteindre les performances attendues.    
    
 Par rapport aux index rowstore traditionnels, les index columnstore permettent d’améliorer jusqu’à 100 fois les performances des charges de travail liées à l’analyse et à l’entreposage des données, et d’obtenir un taux de compression des données jusqu’à 10 fois supérieur. Les index columnstore sont conçus pour rendre le traitement des requêtes beaucoup plus rapide. Ces recommandations vous aident à atteindre les performances attendues. Vous trouverez des explications complémentaires sur les performances des index columnstore à la fin de cet article.    
    
## <a name="recommendations-for-improving-query-performance"></a>Recommandations pour améliorer les performances de requête    
 Voici quelques recommandations pour tirer pleinement parti de tous les avantages des index columnstore.    
    
### <a name="1-organize-data-to-eliminate-more-rowgroups-from-a-full-table-scan"></a>1. Organisez les données pour éliminer davantage de rowgroups dans une analyse de table complète.    
    
-   **Optimisez l’ordre d’insertion.** Dans un entrepôt de données standard, les données sont généralement insérées dans un ordre chronologique et analysées dans la dimension de temps. C’est le cas, par exemple, des analyses de ventes trimestrielles. Pour ce type de charge de travail, l’élimination des rowgroups est automatique. Dans [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)], les rowgroups de nombres peuvent être ignorés dans le processus de traitement des requêtes.    
    
-   **Optimisez l’index cluster rowstore.** Si le prédicat de requête commun se trouve dans une colonne (par exemple, C1) qui n’est pas liée à l’ordre d’insertion de la ligne, vous pouvez créer un index cluster rowstore dans les colonnes C1, puis créer des index cluster columnstore en supprimant l’index cluster rowstore. Si vous créez l’index cluster columnstore explicitement avec `MAXDOP = 1`, l’index cluster columnstore obtenu est parfaitement ordonné dans la colonne C1. Si vous spécifiez `MAXDOP = 8`, vous observez un chevauchement des valeurs entre huit rowgroups. Ce cas se produit souvent quand vous créez l’index columnstore initial pour un jeu de données volumineux. Notez que, dans les index non-cluster columnstore (NCCI), les lignes sont déjà ordonnées si la table de base rowstore a un index cluster. Dans ce cas, l’index non cluster columnstore résultant est automatiquement ordonné. Un point important à retenir est que l’index columnstore ne conserve pas l’ordre des lignes par héritage. Au fur et à mesure que vous ajoutez de nouvelles lignes ou que vous mettez à jour des lignes existantes, vous devrez répéter ce processus si vous constatez une baisse des performances des requêtes analytiques.    
    
-   **Optimisez le partitionnement de table.** Vous pouvez partitionner l’index columnstore, puis utiliser l’élimination de partition pour réduire le nombre de rowgroups à analyser. Par exemple, si vous avez une table de faits dans laquelle sont stockés les achats des clients, un modèle de requête courant est la recherche des achats effectués par un client spécifique par trimestre. Pour cela, vous pouvez combiner l’ordre d’insertion avec le partitionnement sur la colonne client. Dans chaque partition, les lignes sont classées par ordre chronologique pour un client spécifique.    
    
### <a name="2-plan-for-enough-memory-to-create-columnstore-indexes-in-parallel"></a>2. Planifiez suffisamment de mémoire pour créer des index columnstore en parallèle    
 La création d'un index columnstore par défaut est une opération parallèle tant que la mémoire est contrainte. La création de l'index en parallèle requiert plus de mémoire que la création de l'index en série. Lorsqu'il y a suffisamment de mémoire, la création d'un index columnstore prend 1,5 fois plus de temps que créer un arbre B sur les mêmes colonnes.    
    
 La mémoire requise pour créer un index columnstore dépend du nombre de colonnes, du nombre de colonnes de chaîne, du degré de parallélisme (DOP), et des caractéristiques des données. Par exemple, si la table a moins d’un million de lignes, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise un seul thread pour créer l’index columnstore.    
    
 Si la table a plus d’un million de lignes, mais que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne peut pas obtenir suffisamment d’allocation de mémoire pour créer l’index à l’aide de MAXDOP, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] réduit automatiquement la valeur de `MAXDOP` en fonction de l’allocation de mémoire disponible.  Dans certains cas, le DOP doit être réduit à un pour pouvoir créer l'index sous une mémoire contrainte.    
    
 À compter de [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)], la requête s’effectue toujours en mode batch. Dans les versions antérieures, l’exécution en mode batch est uniquement utilisée quand le degré de parallélisme défini est supérieur à un.    
    
## <a name="columnstore-performance-explained"></a>Explication des performances columnstore    
 Les index columnstore optimisent les performances de requête en combinant l’utilisation du mode batch qui accélère le traitement en mémoire avec plusieurs techniques qui réduisent considérablement les E/S nécessaires.  Étant donné que les requêtes d’analyse portent sur un grand nombre de lignes, elles sont généralement dépendantes des E/S. La réduction des E/S pendant l’exécution des requêtes est donc une exigence essentielle dans la conception des index columnstore.  Une fois que les données ont été lues en mémoire, il est primordial de réduire le nombre d’opérations en mémoire.    
    
 Les index columnstore réduisent le nombre d’E/S et optimisent les opérations en mémoire grâce à la forte compression des données, à l’élimination de columnstore et de rowgroup, et au traitement en mode batch.    
    
### <a name="data-compression"></a>Compression des données    
 Les index columnstore offrent un taux de compression des données dix fois supérieur aux index rowstore. Cela réduit sensiblement le nombre d’E/S nécessaires pour l’exécution des requêtes d’analyse, améliorant ainsi les performances de requête.    
    
-   Les index columnstore lisent les données compressées directement sur le disque, ce qui réduit le nombre d’octets de données à lire en mémoire.    
    
-   Les index columnstore stockent les données dans un format compressé en mémoire. Cela réduit le nombre de lectures en mémoire de données identiques et, au final, le nombre d’E/S. Par exemple, avec une compression dix fois plus élevée, les index columnstore peuvent conserver dix fois plus de données en mémoire que si les données étaient stockées dans un format non compressé. Du fait qu’il y ait davantage de données en mémoire, l’index columnstore a plus de chances de trouver les données qu’il recherche dans la mémoire en entraînant des lectures supplémentaires sur le disque.    
    
-   Les index columnstore compressent les données par colonne plutôt que par ligne. C’est ce qui permet d’atteindre des taux de compression élevés et de diminuer le volume des données stockées sur le disque. Chaque colonne est compressée et stockée séparément.  Les données d’une colonne ont toujours le même type et ont souvent des valeurs similaires. Les méthodes de compression de données offrent des taux de compression particulièrement élevés en présence de valeurs similaires.    
    
-   Par exemple, dans une table de faits qui stocke les adresses de clients et qui contient une colonne « pays », le nombre total de valeurs possibles est inférieur à 200. Certaines de ces valeurs sont répétées de nombreuses fois. Si la table de faits contient 100 millions de lignes, les données de la colonne « pays » peuvent être fortement compressées et nécessitent donc très peu de stockage. La compression par ligne ne fonctionne pas sur le même principe de similarité des valeurs de colonne. Elle utilise plus d’octets pour compresser les valeurs de la colonne « pays ».    
    
### <a name="column-elimination"></a>Élimination de colonne    
 Avec les index columnstore, les colonnes qui ne sont pas utiles pour le résultat d’une requête ne sont pas lues. Cette fonction, appelée élimination de colonne, réduit également les E/S nécessaires pour l’exécution d’une requête et améliore ainsi les performances de requête.    
    
-   L’élimination de colonne est possible, car les données sont organisées et compressées par colonne. En revanche, quand les données sont stockées par ligne, les valeurs de colonne dans chaque ligne sont stockées physiquement ensemble et ne peuvent pas être facilement séparées. Le processeur de requêtes doit lire une ligne entière pour récupérer les valeurs de certaines colonnes. Il lit donc inutilement davantage de données en mémoire, ce qui augmente les E/S.    
    
-   Par exemple, si une table contient 50 colonnes et que la requête porte seulement sur cinq de ces colonnes, l’index columnstore récupère uniquement les cinq colonnes en question à partir du disque. L’index ne lit pas les 45 autres colonnes. Cela représente une réduction supplémentaire de 90 % des E/S, en supposant que toutes les colonnes sont de taille similaire. Si les mêmes données étaient stockées dans un rowstore, le processeur de requêtes devrait lire les 45 autres colonnes.    
    
### <a name="rowgroup-elimination"></a>Élimination de rowgroup    
 Dans une analyse de table complète, un grand pourcentage des données n’entre généralement pas dans les critères du prédicat de requête. L’index columnstore utilise des métadonnées pour ignorer les rowgroups qui contiennent des données non pertinentes pour le résultat de la requête, tout cela sans entraîner d’E/S supplémentaires. Cette fonction, appelée élimination de rowgroup, réduit les E/S nécessaires pour l’analyse de tables complètes et, par conséquent, améliore les performances de requête.    
    
 **Quand un index columnstore doit-il effectuer une analyse de table complète ?**    
    
 À compter de [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)], vous pouvez créer un ou plusieurs index non-cluster en arbre B (B-tree) standard sur un index cluster columnstore, comme vous pouvez le faire sur un segment de mémoire rowstore. Les index non-cluster B-tree peuvent accélérer une requête qui est définie avec un prédicat d’égalité ou avec un prédicat comportant une petite plage de valeurs.  Pour les prédicats plus complexes, l’optimiseur de requête peut choisir d’effectuer une analyse de table complète. Sans la fonction d’élimination de rowgroup, l’analyse de table complète serait très longue, surtout pour les tables volumineuses.    
    
 **Quand la fonction d’élimination de rowgroup est-elle intéressante pour une requête d’analyse de table complète ?**    
    
 Prenons l’exemple d’une entreprise de vente au détail qui stocke ses données de vente dans une table de faits ayant un index cluster columnstore. Chaque nouvelle vente est enregistrée avec les différents attributs de la transaction, tels que la date de vente d’un produit. Curieusement, même si l’index columnstore ne garantit pas un ordre de tri, les lignes de cette table sont chargées dans un ordre chronologique. Cette table grossit au fil du temps. L’entreprise de vente au détail conserve peut-être les données de vente des dix dernières années, mais elle peut vouloir effectuer une requête analytique portant uniquement sur un agrégat du dernier trimestre. Les index columnstore peuvent ignorer les données des 39 trimestres précédents en examinant seulement les métadonnées de la colonne « date ». Cela représente une réduction supplémentaire de 97 % du volume des données lues en mémoire et traitées.    
    
 **Quels sont les rowgroups ignorés dans une analyse de table complète ?**    
    
 Pour déterminer les rowgroups à éliminer, l’index columnstore se réfère aux métadonnées pour stocker les valeurs minimale et maximale de chaque segment de colonne pour chaque rowgroup. Si aucun segment de colonne ne correspond à la plage de valeurs définie dans les critères du prédicat de requête, le rowgroup entier est ignoré sans entraîner d’E/S supplémentaires. Ce principe fonctionne, car les données sont généralement chargées dans un ordre trié et, même si les lignes ne sont pas forcément triées, les valeurs de données similaires sont souvent situées dans le même rowgroup ou dans un rowgroup proche.    
    
 Pour plus d’informations sur les rowgroups, consultez Guide des index columnstore.    
    
### <a name="batch-mode-execution"></a>Exécution en mode batch    
 L’exécution en mode batch désigne le fait de traiter simultanément un jeu de lignes, pouvant généralement contenir jusqu’à 900 lignes, pour gagner en efficacité. Par exemple, la requête `SELECT SUM (Sales) FROM SalesData` agrège les ventes totales de la table SalesData. En mode batch, le moteur d’exécution de la requête calcule l’agrégat dans le groupe de 900 valeurs. Cela permet de répartir les coûts d’accès aux métadonnées et d’autres types de traitement entre toutes les lignes du lot, plutôt que de payer les coûts par ligne, réduisant ainsi considérablement le chemin de code. Le traitement en mode batch s’effectue sur les données compressées quand cela est possible et élimine certains opérateurs d’échange utilisés par le traitement en mode ligne. Cette méthode accélère l’exécution des requêtes analytiques par ordre de grandeur.    
    
 Il n’est pas possible d’exécuter tous les opérateurs d’exécution de requête en mode batch. Par exemple, les opérations DML comme Insert, Delete ou Update sont exécutées ligne par ligne. Les opérateurs en mode batch ciblent les opérateurs tels que Scan, Join, Aggregate, Sort, etc. pour améliorer la vitesse de traitement des requêtes. Depuis l’introduction de l’index columnstore dans [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], nous nous efforçons d’étendre la prise en charge d’opérateurs exécutables en mode batch. Le tableau ci-dessous répertorie les opérateurs exécutables en mode batch pour chaque version du produit.    
    
|Opérateurs en mode batch|Contexte d’utilisation|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]|[!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] et [!INCLUDE[ssSDS](../../includes/sssds-md.md)]¹|Commentaires|    
|---------------------------|------------------------|---------------------|---------------------|---------------------------------------|--------------|    
|Opérations DML (insert, delete, update, merge)||non|non|non|DML n’est pas une opération en mode batch, car elle n’est pas effectuée en parallèle. Même si nous rendons possible le traitement batch en mode série, l’ajout du traitement des opérations DML en mode batch n’offre pas d’avantages significatifs.|    
|Columnstore Index Scan|SCAN|N/A|oui|oui|Pour les index columnstore, nous pouvons transmettre le prédicat en mode Push vers le nœud SCAN.|    
|Columnstore Index Scan (non cluster)|SCAN|oui|oui|oui|oui|    
|Index Seek||N/A|N/A|non|Nous effectuons une opération de recherche via un index non-cluster en arbre B (B-tree) en rowmode.|    
|Compute Scalar|Expression ayant pour résultat une valeur scalaire.|oui|oui|oui|Des restrictions s’appliquent au type de données. Cela est vrai pour tous les opérateurs en mode batch.|    
|Concatenation|UNION et UNION ALL|non|oui|oui||    
|Filter|Application de prédicats|oui|oui|oui||    
|Hash Match|Fonctions d’agrégation basées sur le hachage, jointure de hachage externe, jointure de hachage droite, jointure de hachage gauche, jointure interne droite, jointure interne gauche|oui|oui|oui|Restrictions d’agrégation : pas de valeurs min/max pour les chaînes. Les fonctions d’agrégation disponibles sont sum/count/avg/min/max.<br />Restrictions de jointure : pas de jointures sans correspondance de type sur les types non entiers.|    
|merge join||non|non|non||    
|requêtes multithread||oui|oui|oui||    
|boucles imbriquées||non|non|non||    
|requêtes à thread unique exécutées sous MAXDOP 1||non|non|oui||    
|requêtes à thread unique avec un plan de requête série||non|non|oui||    
|Sort|Tri par clause sur SCAN avec l’index columnstore.|non|non|oui||    
|Top Sort||non|non|oui||    
|Window Aggregates||N/A|N/A|oui|Nouvel opérateur dans [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)].|    
    
 ¹S’applique à [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)], aux niveaux Premium, aux niveaux Standard (S3 et ultérieur) et à tous les niveaux vCore [!INCLUDE[ssSDS](../../includes/sssds-md.md)], ainsi qu’à [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]    
    
### <a name="aggregate-pushdown"></a>Agrégation en mode Push    
 Chemin d’exécution standard utilisé pour le calcul d’agrégation qui récupère les lignes qualifiées du nœud SCAN et agrège les valeurs en mode batch. Cette méthode offre de bonnes performances, mais dans [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)], l’opération d’agrégation peut être transmise en mode Push vers le nœud SCAN pour améliorer les performances de calcul d’agrégation par ordre de grandeur avec l’exécution en mode batch. Cela est possible si les conditions suivantes sont remplies : 
 
-    Les agrégats sont `MIN`, `MAX`, `SUM`, `COUNT` et `COUNT(*)`. 
-  L’opérateur d’agrégation doit être au-dessus d’un nœud SCAN ou d’un nœud SCAN avec une clause `GROUP BY`.
-  Cet agrégat n’est pas un agrégat distinct.
-  La colonne d’agrégation n’est pas une colonne de chaîne.
-  La colonne d’agrégation n’est pas une colonne virtuelle. 
-  Le type de données d’entrée et de sortie doit être l’un des types suivants et ne pas dépasser 64 bits.
    -  `tinyint`, `int`, `bigint`, `smallint`, `bit`
    -  `smallmoney`, `money`, `decimal` et `numeric` avec une précision <= 18
    -  `smalldate`, `date`, `datetime`, `datetime2`, `time`
    
 L’agrégation en mode Push est encore plus rapide en exécutant une agrégation sans cache des données compressées/encodées et en utilisant SIMD.    
    
 ![aggregate pushdown](../../relational-databases/indexes/media/aggregate-pushdown.jpg "aggregate pushdown")    
    
Par exemple, une agrégation en mode Push est effectuée dans les deux requêtes ci-dessous :    
    
```sql     
SELECT  productkey, SUM(TotalProductCost)    
FROM FactResellerSalesXL_CCI    
GROUP BY productkey    
    
SELECT  SUM(TotalProductCost)    
FROM FactResellerSalesXL_CCI    
```    
    
### <a name="string-predicate-pushdown"></a>Prédicats de chaîne en mode Push    
Quand vous créez un schéma d’entrepôt de données, le modèle de schéma recommandé est un schéma en étoile ou en flocon contenant une ou plusieurs tables de faits et de nombreuses tables de dimension. La [table de faits](https://en.wikipedia.org/wiki/Fact_table) stocke les mesures ou transactions d’entreprise et la [table de dimension](https://en.wikipedia.org/wiki/Dimension_table) stocke les dimensions sur lesquelles doit porter l’analyse des faits.    
    
Par exemple, un fait est un enregistrement représentant la vente d’un produit particulier dans une région spécifique, tandis que la dimension représente un ensemble de régions, produits, etc. Les tables de faits et de dimension sont associées par une relation de clé primaire/étrangère. Les requêtes analytiques les plus courantes associent une ou plusieurs tables de dimensions avec la table de faits.    
    
Prenons l’exemple d’une table de dimension `Products`. `ProductCode` est une clé primaire classique qui est généralement représentée par un type de données string. Pour améliorer les performances des requêtes, il est recommandé de créer une clé de substitution, généralement une colonne de type integer, pour faire référence à la ligne dans la table de dimension à partir de la table de faits.    
    
L’index columnstore offre de très bonnes performances pour l’exécution de requêtes analytiques avec des jointures/prédicats impliquant des clés numériques ou entières. Toutefois, pour beaucoup de charges de travail client, l’utilisation de colonnes de type string associant des tables de faits/dimension, les performances de requête avec l’index columnstore n’étaient pas aussi bonnes. [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] améliore considérablement les performances des requêtes analytiques sur des colonnes de type string en transmettant en mode Push les prédicats avec les colonnes string vers le nœud SCAN.    
    
Pour améliorer les performances de requête, la transmission Push des prédicats de type string utilise le dictionnaire principal/secondaire créé pour les colonnes. Par exemple, prenons un segment de colonne de type string situé dans un rowgroup de 100 valeurs string distinctes. Cela signifie que chaque valeur string est référencée 10 000 fois sur environ un million de lignes.    
    
Avec la transmission Push des prédicats de type string, la requête calcule le prédicat d’après les valeurs dans le dictionnaire et, en cas d’éligibilité, toutes les lignes faisant référence à la valeur de dictionnaire sont automatiquement qualifiées. Cela améliore les performances de deux manières :
1.  Seule la ligne qualifiée est renvoyée, ce qui réduit le nombre de lignes à transmettre à partir du nœud SCAN. 
2.  Le nombre de comparaisons de chaînes s’en trouve considérablement réduit. Dans cet exemple, seulement 100 chaînes doivent être comparées au lieu d’un million. Les limitations suivantes s’appliquent :    
    -   Il n’y a pas de transmission Push des prédicats de type string pour les rowgroups delta. Il n’existe pas de dictionnaire pour les colonnes des rowgroups delta.    
    -   Il n’y a pas de transmission Push des prédicats de type string si le dictionnaire dépasse la taille de 64 Ko.    
    -   Les expressions ayant pour résultat une valeur NULL ne sont pas prises en charge.    
    
## <a name="see-also"></a> Voir aussi    
 [Index columnstore - Guide de conception](../../relational-databases/indexes/columnstore-indexes-design-guidance.md)   
 [Index columnstore - Conseils en matière de chargement de données](../../relational-databases/indexes/columnstore-indexes-data-loading-guidance.md)   
 [Prise en main de columnstore pour l’analytique opérationnelle en temps réel](../../relational-databases/indexes/get-started-with-columnstore-for-real-time-operational-analytics.md)     
 [Index columnstore pour l’entreposage des données](../../relational-databases/indexes/columnstore-indexes-data-warehouse.md)   
 [Index columnstore - Défragmentation](../../relational-databases/indexes/columnstore-indexes-defragmentation.md)    
 [Index columnstore - Architecture](../../relational-databases/sql-server-index-design-guide.md#columnstore_index)   
 [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)    
 [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md)     
  
