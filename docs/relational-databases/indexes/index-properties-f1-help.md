---
title: Propriétés de l’index – Aide (F1) | Microsoft Docs
ms.custom: ''
ms.date: 02/17/2017
ms.prod: sql
ms.prod_service: table-view-index, sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology: table-view-index
ms.tgt_pltfrm: ''
ms.topic: reference
f1_keywords:
- sql13.swb.indexproperties.filter.f1
- sql13.swb.indexproperties.partitions.f1
- sql13.swb.indexproperties.general.f1
- sql13.swb.indexproperties.storage.f1
- sql13.swb.indexproperties.columns.f1
- sql13.swb.indexproperties.options.f1
- sql13.swb.indexproperties.spatial.f1
ms.assetid: 45efd81a-3796-4b04-b0cc-f3deec94c733
caps.latest.revision: 38
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: a1365d1041a9ceffe9989bb6b859cbeb9d44bd2d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="index-properties-f1-help"></a>Propriétés de l'index – Aide (F1)
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]

  Les sections de cette rubrique font référence aux différentes propriétés d'index disponibles au moyen des boîtes de dialogue [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] .  
  
 **Dans cette rubrique :**  
  
 [Propriétés de l'index (page Général)](#General)  
  
 [Boîte de dialogue Sélectionner les colonnes à partir de (Propriétés de l'index)](#Columns)  
  
 [Propriétés de l'index (page Stockage)](#Storage)  
  
 [Propriétés de l'index (page Spatial)](#Spatial)  
  
 [Propriétés de l'index (page Filtre)](#Filter)  
  
##  <a name="General"></a> Propriétés de l'index (page Général)  
 La page Général vous permet d'afficher et de modifier les propriétés de l'index de la table ou de la vue sélectionnée. Les options de chaque page peuvent changer en fonction du type d'index sélectionné.  
  
 **Nom de la table**  
 Affiche le nom de la table ou de la vue sur laquelle l'index a été créé. Ce champ est en lecture seule. Pour sélectionner une table différente, fermez la page Propriétés de l'index, sélectionnez la table appropriée, puis ouvrez de nouveau la page Propriétés de l'index.  
  
 Des index spatiaux ne peuvent pas être spécifiés sur des vues indexées. Les index spatiaux peuvent être uniquement définis pour une table dotée d'une clé primaire. Le nombre maximal de colonnes clés primaires sur la table est de 15. La taille par ligne combinée des colonnes clés primaires est limitée à 895 octets.  
  
 **Nom de l'index**  
 Affiche le nom de l'index. Ce champ est en lecture seule pour un index existant. Permet de taper le nom de l'index, si vous en créez un nouveau.  
  
 **Type d'index**  
 Indique le type d'index. Pour les nouveaux index, indique le type d'index sélectionné lors de l'ouverture de la boîte de dialogue. Les index peuvent être : **Cluster**, **Non cluster**, **XML primaire**, **XML secondaire**, **Spatial**, **Columnstore cluster**ou **Columnstore non cluster**.  
  
 **Remarque** Un seul index cluster est autorisé pour chaque table. Un seul index columnstore optimisé en mémoire xVelocity est autorisé pour chaque table.  
  
 **Unique**  
 Activez cette case à cocher pour que l'index soit unique. Deux lignes quelconques ne peuvent pas avoir la même valeur d'index. Par défaut, cette case est décochée. Lors de la modification d'un index existant, la création d'index échoue si deux lignes ont la même valeur. Pour les colonnes autorisant les valeurs NULL, un index unique autorise une seule valeur NULL.  
  
 Si vous sélectionnez **Spatial** dans le champ **Type d'index** , la case à cocher **Unique** est estompée.  
  
 **Colonnes clés d'index**  
 Ajoutez les colonnes de votre choix dans la grille **Colonnes de clés d’index** . Lorsque plusieurs colonnes sont ajoutées, elles doivent être répertoriées dans l'ordre souhaité. L'ordre des colonnes dans un index peut avoir un impact important sur les performances de l'index.  
  
 Un maximum de 16 colonnes peuvent être utilisées dans un index composé individuel. Si la valeur est supérieure à 16 colonnes, consultez la section traitant des colonnes incluses à la fin de cette rubrique.  
  
 Un index spatial peut être uniquement défini sur une colonne unique qui contient un type de données spatiales (une *colonne spatiale*).  
  
 **Nom**  
 Affiche le nom de la colonne utilisée dans la clé d'index.  
  
 **Ordre de tri**  
 Spécifie le sens du tri de la colonne d’index sélectionnée, **Croissant** ou **Décroissant**.  
  
> [!NOTE]  
>  Si le type d’index est **XML primaire** ou **Spatial**, cette colonne n’apparaît pas dans la table.  
  
 **Type de données**  
 Affiche les informations de type de données.  
  
> [!NOTE]  
>  Si la colonne de table est une colonne calculée, le paramètre **Type de données** a la valeur « colonne calculée ».  
  
 **Taille**  
 Affiche le nombre maximal d'octets requis pour stocker le type de données de la colonne. Affiche zéro (0) pour une colonne spatiale ou XML.  
  
 **Identity**  
 Indique si la colonne utilisée dans la clé d'index est une colonne d'identité.  
  
 **Autoriser les valeurs NULL**  
 Indique si la colonne utilisée dans la clé d'index autorise le stockage de valeurs NULL dans la colonne de la table ou de la vue.  
  
 **Ajouter**  
 Ajoute une colonne à la clé d'index. Sélectionnez des colonnes de table dans la boîte de dialogue **Sélectionnez les colonnes dans** *\<nom_table>* qui s’affiche quand vous cliquez sur **Ajouter**. Pour un index spatial, lorsque vous sélectionnez une colonne, ce bouton est estompé.  
  
 **Supprimer**  
 Supprime la colonne sélectionnée de la clé d'index.  
  
 **Monter**  
 Déplace la colonne sélectionnée vers le haut dans la grille de clé d'index.  
  
 **Descendre**  
 Déplace la colonne sélectionnée vers le bas dans la grille de clé d'index.  
  
 **Colonnes columnstore**  
 Cliquez sur **Ajouter** pour sélectionner des colonnes pour l’index columnstore. Pour connaître les limitations sur un index columnstore, consultez [CREATE COLUMNSTORE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-columnstore-index-transact-sql.md).  
  
 **Colonnes incluses**  
 Inclut des colonnes non-clés dans l'index non cluster. Cette option vous permet de contourner les limites actuelles de l'index relatives à la taille totale d'une clé d'index et au nombre maximal de colonnes pouvant faire partie d'une clé d'index, en ajoutant des colonnes en tant que colonnes non-clés au niveau feuille de l'index non cluster. Pour plus d’informations, consultez [Créer des index avec colonnes incluses](../../relational-databases/indexes/create-indexes-with-included-columns.md)  
  
##  <a name="Columns"></a> Boîte de dialogue Sélectionner les colonnes à partir de (Propriétés de l'index)  
 Utilisez cette page pour ajouter des colonnes à la page **Propriétés de l'index (page Général)** lors de la création ou de la modification d'un index.  
  
 **Case à cocher**  
 Activez pour ajouter des colonnes.  
  
 **Nom**  
 Nom de la colonne.  
  
 **Type de données**  
 Type de données de la colonne.  
  
 **Octets**  
 Taille de la colonne en octets.  
  
 **Identity**  
 Affiche **Oui** pour les colonnes d’identité et **Non** pour les autres.  
  
 **Allow Nulls**  
 Affiche **Oui** si la définition de table autorise la présence de valeurs Null dans la colonne. Affiche **Non** si la définition de la table interdit la présence de valeurs Null dans la colonne.  
  
##  <a name="Storage"></a> Options (page Stockage)  
 Utilisez cette page pour consulter ou modifier les propriétés de groupe de fichiers ou de schéma de partition pour l'index sélectionné. Affiche uniquement les options relatives au type d'index.  
  
 **Groupe de fichiers**  
 Stocke l'index dans le groupe de fichiers spécifié. La liste répertorie uniquement les groupes de fichiers standard (row). Le groupe de fichiers PRIMARY de la base de données est sélectionné par défaut dans la liste. Pour plus d'informations, consultez [Database Files and Filegroups](../../relational-databases/databases/database-files-and-filegroups.md).  
  
 **Groupe de fichiers Filestream**  
 Spécifie le groupe de fichiers pour les données FILESTREAM. Cette liste affiche uniquement des groupes de fichiers FILESTREAM. Le groupe de fichiers sélectionné par défaut dans la liste est le groupe PRIMARY FILESTREAM. Pour plus d’informations, consultez [FILESTREAM &#40;SQL Server&#41;](../../relational-databases/blob/filestream-sql-server.md).  
  
 **Schéma de partition**  
 Stocke l'index dans un schéma de partition. En cliquant sur **Schéma de partition** , vous activez la grille ci-dessous. Le schéma de partition sélectionné par défaut dans la liste est celui qui est utilisé pour stocker les données de la table. Si vous sélectionnez un autre schéma de partition dans la liste, les informations affichées dans la grille sont actualisées. Pour plus d’informations, consultez [Tables et index partitionnés](../../relational-databases/partitions/partitioned-tables-and-indexes.md).  
  
 L'option Schéma de partition n'est pas disponible s'il n'y a pas de schémas de partition dans la base de données.  
  
 **Schéma de partition Filestream**  
 Spécifie le schéma de partition utilisé pour les données FILESTREAM. Ce schéma de partition doit être symétrique avec celui spécifié dans l'option **Schéma de partition** .  
  
 Si la table n'est pas partitionnée, le champ est vide.  
  
 **Paramètre du schéma de partition**  
 Affiche le nom de la colonne qui participe au schéma de partition.  
  
 **Colonne de table**  
 Sélectionnez la table ou vue à mapper avec le schéma de partition.  
  
 **Type de données de la colonne**  
 Affiche des informations sur les types de données figurant dans la colonne.  
  
> [!NOTE]  
>  Si la colonne de table est une colonne calculée, **Type de données de la colonne** contient la mention « colonne calculée ».  
  
 **Autoriser le traitement en ligne des instructions DML lors du déplacement de l'index**  
 Permet aux utilisateurs d'accéder à la table sous-jacente ou aux données des index cluster et à tous les index non-cluster associés pendant l'opération d'index. Pour plus d'informations, consultez [Perform Index Operations Online](../../relational-databases/indexes/perform-index-operations-online.md).  
  
> [!NOTE]  
>  Cette option n'est pas disponible pour les index XML ou si l'index est un index cluster désactivé.  
  
 **Définir le degré maximal de parallélisme**  
 Limite le nombre de processeurs à utiliser pendant l'exécution des plans parallèles. La valeur par défaut est 0, indiquant que le nombre réel de processeurs disponibles est utilisé. Spécifier la valeur 1 supprime la génération d'un plan parallèle ; spécifier une valeur supérieure à 1 limite le nombre maximal de processeurs utilisés au cours de l'exécution d'une requête simple. Cette option est disponible seulement si la boîte de dialogue est dans l'état **Reconstruire** ou **Recréer** . Pour plus d'informations, consultez [Set the Max Degree of Parallelism Option for Optimal Performance](../../relational-databases/policy-based-management/set-the-max-degree-of-parallelism-option-for-optimal-performance.md).  
  
> [!NOTE]  
>  Si une valeur supérieure au nombre d'UC disponibles est spécifiée, le nombre réel d'UC est utilisé.  
  
##  <a name="Spatial"></a> Options de l'index (page Spatial)  
 Utilisez la page **Spatial** pour afficher ou spécifier les valeurs des propriétés spatiales. Pour plus d’informations, consultez [Données spatiales &#40;SQL Server&#41;](../../relational-databases/spatial/spatial-data-sql-server.md).  
  
### <a name="bounding-box"></a>Cadre englobant  
 Le *cadre englobant* est le périmètre de la grille de niveau supérieur d’un plan géométrique. Les paramètres du cadre englobant existent uniquement dans le pavage de la grille géométrique. Ces paramètres sont indisponibles si le **Schéma de pavage** est une **Grille géographique**.  
  
 Le panneau affiche les coordonnées **(***Min. X***,***Min. Y***)** et **(***Max. X***,***Max. Y***)** du cadre englobant. Il n'y a pas de valeurs de coordonnées par défaut. Par conséquent, quand vous créez un index spatial sur une colonne de type **geometry** , vous devez spécifier les valeurs des coordonnées.  
  
 **Min. X**  
 Coordonnée X de l'angle inférieur gauche du cadre englobant.  
  
 **Min. Y**  
 Coordonnée Y de l'angle inférieur gauche du cadre englobant.  
  
 **Max. X**  
 Coordonnée X de l'angle supérieur droit du cadre englobant.  
  
 **Max. Y**  
 Coordonnée Y de l'angle supérieur droit du cadre englobant.  
  
### <a name="general"></a>Général  
 **Schéma de pavage**  
 Indique le schéma de pavage de l'index. Les schémas de pavage pris en charge se présentent comme suit.  
  
 **Grille géométrique**  
 Spécifie le schéma de pavage de la grille géométrique qui s’applique à une colonne du type de données **geometry** .  
  
 **Grille automatique géométrique**  
 Cette option est activée pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] quand le niveau de compatibilité de la base de données a la valeur 110 ou une valeur supérieure.  
  
 **Grille géographique**  
 Spécifie le schéma de pavage de la grille géographique qui s’applique à une colonne du type de données **geography** .  
  
 **Grille automatique géographique**  
 Cette option est activée pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] quand le niveau de compatibilité de la base de données a la valeur 110 ou une valeur supérieure.  
  
 Pour plus d’informations sur la façon dont [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] implémente le pavage, consultez [Données spatiales &#40;SQL Server&#41;](../../relational-databases/spatial/spatial-data-sql-server.md).  
  
 **Cellules par objet**  
 Indique le nombre de cellules par objet de pavage qui peuvent être utilisées pour un objet spatial unique dans l'index. Ce nombre peut être un entier compris entre 1 et 8192 (inclus). La valeur par défaut est 16, et 8 pour les versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] lorsque le niveau de compatibilité de la base de données est défini sur 110 ou ultérieur.  
  
 Au niveau supérieur, si un objet couvre plus de cellules que le nombre spécifié par *n*, l’indexation utilise autant de cellules que nécessaire pour fournir un pavage de niveau supérieur complet. Dans de tels cas, un objet peut recevoir plus de cellules que le nombre spécifié. Dans ce cas, le nombre maximal est le nombre de cellules générées par la grille de niveau supérieur, qui dépend de la densité du **Niveau 1** .  
  
### <a name="grids"></a>Grilles  
 Ce panneau affiche la densité de la grille à chaque niveau du schéma de pavage. La densité est spécifiée comme suit : **Basse**, **Moyenne**ou **Haute**. La valeur par défaut est **Moyenne**. **Basse** représente une grille 4x4 (16 cellules), **Moyenne** une grille 8x8 (64 cellules) et **Haute** une grille 16x16 (256 cellules). Ces options ne sont pas disponibles lorsque les options de pavage **Grille automatique géométrique** ou **Grille automatique géographique** sont choisies.  
  
 **Niveau 1**  
 Densité de la grille de premier niveau (haut).  
  
 **Niveau 2**  
 Densité de la grille de second niveau.  
  
 **Niveau 3**  
 Densité de la grille de troisième niveau.  
  
 **Niveau 4**  
 Densité de la grille de quatrième niveau.  
  
##  <a name="Filter"></a> Page Filtre  
 Utilisez cette page pour entrer le prédicat de filtre d'un index filtré. Pour plus d'informations, consultez [Create Filtered Indexes](../../relational-databases/indexes/create-filtered-indexes.md).  
  
 **Expression de filtre**  
 Définit quelles lignes de données inclure dans l'index filtré. Par exemple : `StartDate > '20000101' AND EndDate IS NOT NULL'.`  
  
## <a name="see-also"></a> Voir aussi  
 [Définir les options d'index](../../relational-databases/indexes/set-index-options.md)   
 [INDEXPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/indexproperty-transact-sql.md)   
 [sys.indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md)  
  
  
