---
title: Créer, modifier et supprimer les index spatiaux | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: spatial
ms.reviewer: ''
ms.suite: sql
ms.technology:
- dbe-spatial
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- indexes [SQL Server], creating
- spatial indexes [SQL Server], dropping
- spatial indexes [SQL Server], creating
- indexes [SQL Server], dropping
- indexes [SQL Server], modifying
- spatial indexes [SQL Server], modifying
ms.assetid: 00c1b927-8ec5-44cf-87c2-c8de59745735
caps.latest.revision: 23
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 7466a59f0f9ae3eed8a348cffc43eea519763856
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-modify-and-drop-spatial-indexes"></a>Créer, modifier et supprimer les index spatiaux
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  Un index spatial peut effectuer plus efficacement certaines opérations sur une colonne du type de données **geometry** ou **geography** (une *colonne spatiale*). Plusieurs index spatiaux peuvent être spécifiés sur une colonne spatiale. Cela peut s'avérer utile par exemple pour indexer différents paramètres de pavage dans une même colonne.  
  
 Il existe plusieurs restrictions applicables à la création d'index spatiaux. Pour plus d'informations, consultez [Restrictions sur les index spatiaux](#restrictions) dans cette rubrique.  
  
> [!NOTE]  
>  Pour plus d’informations sur la relation entre les index spatiaux et les partitions et groupes de fichiers, consultez la section « Remarques » dans [CREATE SPATIAL INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-spatial-index-transact-sql.md).  
  
##  <a name="creating"></a> Création, modification et suppression d'index spatiaux  
  
###  <a name="create"></a> Pour créer un index spatial  
 **Pour créer un index spatial à l'aide de Transact-SQL**  
 [CREATE SPATIAL INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-spatial-index-transact-sql.md)  
  
 **Pour créer un index spatial à l'aide de la boîte de dialogue Nouvel index dans Management Studio**  
 ##### <a name="to-create-a-spatial-index-in-management-studio"></a>Pour créer un index spatial dans Management Studio  
  
1.  Dans l'Explorateur d'objets, connectez-vous à une instance du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] et développez-la.  
  
2.  Développez **Bases de données**, développez la base de données qui contient la table dotée de l'index spécifié, puis développez **Tables**.  
  
3.  Développez la table pour laquelle vous souhaitez créer l'index.  
  
4.  Cliquez avec le bouton droit sur **Index** et sélectionnez **Nouvel index**.  
  
5.  Dans le champ **Nom de l'index** , entrez un nom pour l'index.  
  
6.  Dans la liste déroulante **Type d’index** , sélectionnez **Spatial**.  
  
7.  Pour spécifier la colonne spatiale à indexer, cliquez sur **Ajouter**.  
  
8.  Dans la boîte de dialogue **Sélectionner des colonnes à partir de** *\<nom_table>*, sélectionnez une colonne de type **geometry** ou **geography** en cochant la case correspondante. Toutes les autres colonnes spatiales deviennent alors impossibles à modifier. Si vous souhaitez sélectionner une autre colonne spatiale, vous devez tout d'abord désactiver la colonne sélectionnée actuellement. Lorsque vous avez terminé, cliquez sur **OK**.  
  
9. Vérifiez votre sélection de colonne dans la grille **Colonnes clés d'index** .  
  
10. Dans le volet **Sélectionner une page** de la boîte de dialogue **Propriétés de l'index** , cliquez sur **Spatial**.  
  
11. Dans la page **Spatial** , spécifiez les valeurs que vous souhaitez utiliser pour les propriétés spatiales de l'index.  
  
     Quand vous créez un index sur une colonne de type **geometry**, vous devez spécifier les coordonnées **(***Min. X***,***Min. Y***)** et **(***Max. X***,***Max. Y***)** du cadre englobant. Pour un index sur une colonne de type **geography** , les champs de cadre englobant deviennent en lecture seule après que vous avez spécifié le schéma de pavage **Grille géographique** , car le pavage de la grille de géographie n’utilise pas de cadre englobant.  
  
     Si vous le souhaitez, vous pouvez spécifier des valeurs autres que les valeurs par défaut pour le champ **Cellules par objet** et pour la densité de grille à tout niveau du schéma de pavage. La quantité par défaut de cellules par objet est 16 pour [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] ou 8 pour [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] ou les versions supérieures, et la densité de grille par défaut est **Moyenne** pour [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)].  
  
     Vous pouvez sélectionner GEOMETRY_AUTO_GRID ou GEOGRAPHY_AUTO_GRID pour le schéma de pavage dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Lorsque GEOMETRY_AUTO_GRID ou GEOGRAPHY_AUTO_GRID est sélectionné, les options de densité de la grille Niveau 1, Niveau 2, Niveau 3 et Niveau 4 sont désactivées.  
  
     Pour plus d'informations sur ces propriétés, consultez [Index Properties F1 Help](../../relational-databases/indexes/index-properties-f1-help.md).  
  
12. Cliquez sur **OK**.  
  
> [!NOTE]  
>  Pour créer un autre index spatial sur la même colonne spatiale ou sur une colonne spatiale différente, répétez les étapes précédentes.  
  
  
 **Pour créer un index spatial à l'aide du Concepteur de tables dans Management Studio**  
 ##### <a name="to-create-a-spatial-index-in-table-designer"></a>Pour créer un index spatial dans le Concepteur de tables  
  
1.  Dans l’Explorateur d’objets, cliquez avec le bouton droit sur la table pour laquelle vous souhaitez créer un index spatial, puis cliquez sur **Conception**.  
  
     La table s'ouvre dans le Concepteur de tables.  
  
2.  Sélectionnez une colonne **geometry** ou **geography** pour l'index.  
  
3.  Dans le menu **Concepteur de tables** , cliquez sur **Index spatial**.  
  
4.  Dans la boîte de dialogue **Index spatiaux** , cliquez sur **Ajouter**.  
  
5.  Sélectionnez le nouvel index dans la liste **Index spatial sélectionné** et, dans la grille située à droite, définissez les propriétés de l'index spatial. Pour plus d’informations sur les propriétés, consultez [Boîte de dialogue Index spatiaux &#40;Visual Database Tools&#41;](http://msdn.microsoft.com/library/4d84239a-68c7-4aa2-8602-2b51dd07260f).  
  
  
###  <a name="alter"></a> Pour modifier un index spatial  
  
-   [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md)  
  
    > [!IMPORTANT]  
    >  Pour modifier des options spécifiques à un index spatial, telles que BOUNDING_BOX ou GRID, vous pouvez utiliser une instruction CREATE SPATIAL INDEX qui spécifie DROP_EXISTING = ON ou supprimer l'index spatial et en créer un nouveau. Pour obtenir un exemple, consultez [CREATE SPATIAL INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-spatial-index-transact-sql.md).  
  
-   [Modifier un index](../../relational-databases/indexes/modify-an-index.md)  
  
-   [Déplacer un index existant dans un autre groupe de fichiers](../../relational-databases/indexes/move-an-existing-index-to-a-different-filegroup.md)  
  
  
###  <a name="drop"></a> Pour supprimer un index spatial  
 **Pour supprimer un index spatial à l'aide de Transact-SQL**  
 [DROP INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/drop-index-transact-sql.md)  
  
 **Pour supprimer un index à l'aide de Management Studio**  
 [Supprimer un index](../../relational-databases/indexes/delete-an-index.md)  
  
 **Pour supprimer un index spatial à l'aide du Concepteur de tables dans Management Studio**  
 ##### <a name="to-drop-a-spatial-index-in-table-designer"></a>Pour supprimer un index spatial dans le Concepteur de tables  
  
1.  Dans l’Explorateur d’objets, cliquez avec le bouton droit sur la table contenant l’index spatial que vous souhaitez supprimer et cliquez sur **Conception**.  
  
     La table s'ouvre dans le Concepteur de tables.  
  
2.  Dans le menu **Concepteur de tables** , cliquez sur **Index spatial**.  
  
     La boîte de dialogue **Index spatial** s'ouvre.  
  
3.  Cliquez sur l'index que vous souhaitez supprimer dans la colonne **Index spatial sélectionné** .  
  
4.  Cliquez sur **Supprimer**.  
  
  
##  <a name="restrictions"></a> Restrictions sur les index spatiaux  
 Un index spatial peut être créé uniquement sur une colonne de type **geometry** ou **geography**.  
  
### <a name="table-and-view-restrictions"></a>Restrictions sur les tables et les vues  
 Les index spatiaux peuvent être définis uniquement sur une table dotée d'une clé primaire. Le nombre maximal de colonnes clés primaires sur la table est de 15.  
  
 La taille maximale des enregistrements de clés d'index est de 895 octets. Les tailles supérieures génèrent une erreur.  
  
> [!NOTE]  
>  Les métadonnées de clé primaire ne peuvent pas être modifiées pendant qu'un index spatial est défini sur une table.  
  
 Des index spatiaux ne peuvent pas être spécifiés sur des vues indexées.  
  
### <a name="multiple-spatial-index-restrictions"></a>Restrictions sur plusieurs index spatiaux  
 Vous pouvez créer jusqu'à 249 index spatiaux sur les colonnes spatiales dans une table prise en charge. La création de plusieurs index spatiaux sur la même colonne spatiale peut être utile, par exemple pour indexer des paramètres de pavage différents dans une même colonne.  
  
 Vous pouvez créer un seul index spatial à la fois.  
  
### <a name="spatial-indexes-and-process-parallelism"></a>Index spatiaux et parallélisme de processus  
 Une construction d'index peut utiliser le parallélisme de processus disponible.  
  
### <a name="version-restrictions"></a>Restrictions de version  
 Les pavages spatiaux introduits dans [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] ne peuvent pas être répliqués dans [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] ou [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]. Vous devez utiliser les pavages spatiaux [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] ou [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] pour les index spatiaux lorsqu'une compatibilité descendante avec les bases de données [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] ou [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] est une condition obligatoire.  
  
  
## <a name="see-also"></a> Voir aussi  
 [Vue d’ensemble des index spatiaux](../../relational-databases/spatial/spatial-indexes-overview.md)  
  
  
