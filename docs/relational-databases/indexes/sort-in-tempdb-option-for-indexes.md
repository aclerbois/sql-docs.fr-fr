---
title: Option SORT_IN_TEMPDB pour les index | Microsoft Docs
ms.custom: ''
ms.date: 04/24/2017
ms.prod: sql
ms.prod_service: table-view-index, sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology: table-view-index
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- SORT_IN_TEMPDB option
- disk space [SQL Server], indexes
- space [SQL Server], indexes
- tempdb database [SQL Server], indexes
- indexes [SQL Server], tempdb database
- index creation [SQL Server], tempdb database
ms.assetid: 754a003f-fe51-4d10-975a-f6b8c04ebd35
caps.latest.revision: 26
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 71d1ac448c2dce243fe8d8b48603f26c84331d8c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sortintempdb-option-for-indexes"></a>Option SORT_IN_TEMPDB pour les index
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]

  Quand vous créez ou reconstruisez un index, vous pouvez, en affectant la valeur ON à l’option SORT_IN_TEMPDB, demander au [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] d’utiliser la base de données **tempdb** pour stocker les résultats de tri intermédiaires qui sont utilisés pour générer l’index. Bien que cette option augmente la quantité d’espace disque temporaire utilisé pour la création d’un index, elle peut réduire le temps nécessaire à la création ou à la reconstruction d’un index lorsque la base de données **tempdb** ne se trouve pas sur le même ensemble de disques que la base de données utilisateur. Pour plus d’informations sur **tempdb**, consultez [Configurer l’option de configuration Création d’index en mémoire](../../database-engine/configure-windows/configure-the-index-create-memory-server-configuration-option.md).  
  
## <a name="phases-of-index-building"></a>Phases de la construction d'un index  
 La construction d'un index par le [!INCLUDE[ssDE](../../includes/ssde-md.md)] comprend les phases suivantes :  
  
-   Tout d'abord, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] analyse les pages de données de la table de base pour récupérer les valeurs des clés et construire une ligne d'index de niveau feuille pour chaque ligne de données. Lorsque les tampons de tri internes ont été remplis avec des entrées d'index de niveau feuille, les entrées sont triées et écrites sur le disque sous forme d'exécution triée intermédiaire. Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] reprend ensuite l'analyse des pages de données jusqu'à ce que les tampons de tri soient de nouveau remplis. Ce modèle d'analyse de plusieurs pages de données, suivi d'un tri et d'une écriture d'exécution triée, se poursuit jusqu'à ce que toutes les lignes de la table de base de données aient été traitées.  
  
     Dans un index cluster, les lignes du niveau feuille de l'index sont les lignes de données de la table, c'est pourquoi les tris intermédiaires contiennent toutes les lignes de données. Dans un index non-cluster, les lignes de niveau feuille peuvent contenir des colonnes non-clés, mais sont généralement plus petites que dans un index cluster. Si les clés d'index sont longues, ou si l'index contient plusieurs colonnes non-clés, un tri non-cluster peut retourner beaucoup de lignes. Pour plus d’informations sur l’inclusion de colonnes non-clés, consultez [Créer des index avec colonnes incluses](../../relational-databases/indexes/create-indexes-with-included-columns.md).  
  
-   Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] fusionne les tris des lignes du niveau feuille de l'index en un flux unique trié. Le composant de fusion et tri du [!INCLUDE[ssDE](../../includes/ssde-md.md)] commence par la première page de chaque tri, recherche la clé la plus faible dans toutes les pages et passe cette ligne feuille au composant de création d'index. La clé la plus faible suivante est ensuite traitée, puis la suivante et ainsi de suite. Lorsque la dernière ligne feuille de l'index est extraite d'une page d'exécution triée, le processus passe à la page suivante à partir de cette exécution triée. Lorsque toutes les pages d'une extension à exécution triée ont été traitées, l'extension est libérée. Lorsque chaque ligne feuille de l'index est passée au composant de création d'index, elle est placée dans une page feuille de l'index dans le tampon. Chaque page feuille est écrite au moment de son remplissage. Lors de l'écriture des pages de niveau feuille, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] construit également les niveaux supérieurs de l'index. Chaque page d'index de niveau supérieur est écrite lorsqu'elle est remplie.  
  
## <a name="sortintempdb-option"></a>option SORT_IN_TEMPDB  
 Lorsque l'option SORT_IN_TEMPDB est désactivée, ce qui constitue l'option par défaut, les tris sont stockés dans le groupe de fichiers de destination. Pendant la première phase de création de l'index, les lectures des pages de la table de base et les écritures des exécutions triées déplacent les têtes de lecture-écriture du disque d'une zone du disque à l'autre. Les têtes se trouvent dans la zone de la page de données lors de l'analyse des pages de données. Elles passent dans une zone d'espace libre lorsque les tampons de tri se remplissent et que le tri en cours doit être écrit sur le disque, puis reviennent sur la zone de la page de données lorsque l'analyse des pages de la table reprend. Le mouvement de la tête de lecture-écriture est plus important dans la deuxième phase. À ce stade, le processus de tri alterne généralement les lectures de chaque zone d'exécution triée. Les exécutions triées et les nouvelles pages d'index sont construites dans le groupe de fichiers de destination. Ainsi, lorsque le [!INCLUDE[ssDE](../../includes/ssde-md.md)] répartit les lectures entre les différents tris, il doit en même temps accéder périodiquement aux extensions d'index pour écrire les nouvelles pages d'index à mesure qu'elles sont remplies.  
  
 Si l’option SORT_IN_TEMPDB est activée et que **tempdb** ne se trouve pas dans le même ensemble de disques que le groupe de fichiers de destination, les lectures des pages de données pendant la première phase ne sont pas effectuées sur le même disque que les écritures dans la zone de travail de tri dans **tempdb**. Cela signifie que les lectures des clés de données se poursuivent davantage en série sur le disque, tout comme les écritures sur le disque **tempdb** et les écritures qui construisent l’index final. Même si d'autres utilisateurs utilisent la base de données et accèdent à des adresses de disque séparées, le modèle global de lecture et d'écriture est plus efficace lorsque SORT_IN_TEMPDB est spécifié que lorsqu'il ne l'est pas.  
  
 L'option SORT_IN_TEMPDB peut améliorer la contiguïté des extensions d'index, surtout si l'opération CREATE INDEX n'est pas traitée en parallèle. Les extensions de la zone de travail de tri sont libérées de façon quelque peu aléatoire par rapport à leur emplacement dans la base de données. Si les zones de travail de tri sont contenues dans le groupe de fichiers de destination, les extensions qui se libèrent peuvent être acquises par les demandes d'extensions pour le stockage de la structure de l'index pendant sa construction. Ceci peut rendre les emplacements des extensions d'index aléatoires jusqu'à un certain point. Si les extensions de tri sont contenues séparément dans **tempdb**, la séquence selon laquelle elles sont libérées n’a aucune incidence sur l’emplacement des extensions d’index. En outre, lorsque les tris intermédiaires sont stockés dans **tempdb** et pas dans le groupe de fichiers de destination, l’espace disponible dans ce dernier est plus important. Cela augmente les chances de contiguïté des extensions d'index.  
  
 L'option SORT_IN_TEMPDB affecte uniquement l'instruction en cours. Aucune métadonnée n’enregistre le fait que l’index a ou n’a pas été trié dans **tempdb**. Par exemple, si vous créez un index non-cluster avec l'option SORT_IN_TEMPDB et que, par la suite, vous créez un index cluster sans cette option, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] n'utilise pas l'option lorsqu'il recrée l'index cluster.  
  
> [!NOTE]  
>  Si l'opération de tri n'est pas nécessaire ou peut avoir lieu dans la mémoire, l'option SORT_IN_TEMPDB est ignorée.  
  
## <a name="disk-space-requirements"></a>Espace disque nécessaire  
 Lorsque l’option SORT_IN_TEMPDB est activée, vous devez avoir suffisamment d’espace libre dans **tempdb** pour contenir les tris intermédiaires, et suffisamment d’espace libre dans le groupe de fichiers de destination pour contenir le nouvel index. L'instruction CREATE INDEX échoue s'il n'y a pas suffisamment d'espace libre et si, pour une raison quelconque, les bases de données ne peuvent pas croître automatiquement pour acquérir plus d'espace (notamment s'il n'y a pas d'espace sur le disque ou si la fonction de croissance automatique est désactivée).  
  
 Si l'option SORT_IN_TEMPDB n'est pas activée, l'espace libre disponible dans le groupe de fichiers de destination doit correspondre environ à la taille de l'index final. Pendant la première phase, les exécutions triées sont construites et nécessitent environ la même quantité d'espace que l'index final. Pendant la deuxième phase, chaque extension comportant une exécution triée est libérée après avoir été traitée. Cela signifie que les extensions de tri sont libérées à peu près à la même vitesse que des extensions sont acquises pour le stockage des pages de l'index final et, de ce fait, l'espace nécessaire global ne dépasse pas beaucoup la taille de l'index final. Ceci présente toutefois l'inconvénient suivant : si la quantité d'espace libre est très proche de la taille de l'index final, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] tend à réutiliser les extensions de tri très rapidement après leur libération. Puisque ces dernières sont libérées d'une façon quelque peu aléatoire, la continuité des extensions d'index dans ce scénario est réduite. Lorsque l'option SORT_IN_TEMPDB est désactivée, la continuité des extensions d'index est améliorée s'il y a, dans le groupe de fichiers de destination, suffisamment d'espace libre pour permettre l'allocation des extensions d'index à partir d'un pool contigu et non à partir des extensions de tri qui viennent d'être libérées.  
  
 Lorsque vous créez un index non-cluster, vous devez respecter les règles suivantes en matière d'espace libre :  
  
-   Si l’option SORT_IN_TEMPDB est activée, il doit y avoir suffisamment d’espace libre dans **tempdb** pour stocker les tris, et suffisamment d’espace dans le groupe de fichiers de destination pour stocker la structure finale de l’index. Les exécutions triées contiennent les lignes feuilles de l'index.  
  
-   Si l'option SORT_IN_TEMPDB n'est pas activée, l'espace libre dans le groupe de fichiers de destination doit être suffisamment grand pour stocker la structure finale de l'index. La continuité des extensions d'index peut être améliorée lorsque l'espace libre est important.  
  
 Lorsque vous créez un index cluster sur une table qui n'a pas d'index non-cluster, vous devez respecter les règles suivantes en matière d'espace libre :  
  
-   Si l’option SORT_IN_TEMPDB est activée, il doit y avoir suffisamment d’espace libre dans **tempdb** pour stocker les tris. Ceux-ci comprennent les lignes de données de la table. Il doit y avoir suffisamment d'espace libre dans le groupe de fichiers de destination pour stocker la structure de l'index final. Ceci comprend les lignes de données de la table et l'arborescence binaire de l'index. Il se peut que vous deviez ajuster l'estimation en fonction de facteurs tels que le fait d'avoir une taille de clé élevée ou un faible taux de remplissage.  
  
-   Si l'option SORT_IN_TEMPDB n'est pas activée, l'espace libre dans le groupe de fichiers de destination doit être suffisamment grand pour stocker la table finale. Ceci comprend la structure de l'index. La continuité de la table et des extensions d'index peut être améliorée si l'espace libre disponible est plus important.  
  
 Lorsque vous créez un index cluster sur une table qui a des index non-cluster, vous devez respecter les règles suivantes en matière d'espace libre :  
  
-   Si l’option SORT_IN_TEMPDB est activée, il doit y avoir suffisamment d’espace libre dans **tempdb** pour stocker la collection de tris pour le plus grand index, généralement l’index cluster, et suffisamment d’espace dans le groupe de fichiers de destination pour stocker les structures finales de tous les index. Ceci comprend l'index cluster qui contient les lignes de données de la table.  
  
-   Si l'option SORT_IN_TEMPDB n'est pas activée, l'espace libre dans le groupe de fichiers de destination doit être suffisamment grand pour stocker la table finale. Ceci comprend les structures de tous les index. La continuité de la table et des extensions d'index peut être améliorée si l'espace libre disponible est plus important.  
  
## <a name="related-tasks"></a>Related Tasks  
 [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)  
  
 [Réorganiser et reconstruire des index](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md)  
  
## <a name="related-content"></a>Contenu associé  
 [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md)  
  
 [Configurer l’option de configuration Création d’index en mémoire.](../../database-engine/configure-windows/configure-the-index-create-memory-server-configuration-option.md)  
  
 [Espace disque nécessaire pour les opérations DDL d'index](../../relational-databases/indexes/disk-space-requirements-for-index-ddl-operations.md)  
  
  
