---
title: Ajouter ou modifier une jointure | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: replication
ms.reviewer: ''
ms.suite: sql
ms.technology:
- replication
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.rep.newpubwizard.addeditjoin.f1
ms.assetid: 3b546560-720f-48b8-9d63-cf159290e9d4
caps.latest.revision: 25
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 83ddc6ebe767289ad3704056ae04caf9ed509329
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="add-or-edit-join"></a>Ajouter ou modifier une jointure
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Les boîtes de dialogue **Ajouter une jointure** et **Modifier une jointure** permettent d'ajouter et de modifier des filtres de jointure pour les publications de fusion.  
  
> [!NOTE]  
>  Pour pouvoir modifier un filtre dans une publication existante, vous devez procéder au préalable à un nouvel instantané de la publication. Si cette dernière dispose d'abonnements, ces abonnements doivent alors être réinitialisés. Pour plus d’informations sur les modifications de propriétés, consultez [Changer des propriétés de publication et d’article](../../relational-databases/replication/publish/change-publication-and-article-properties.md).  
  
 Un filtre de jointure permet de filtrer une table en fonction du filtrage d'une table associée dans la publication. En règle générale, une table parente est filtrée en utilisant un filtre de lignes paramétrable. Ensuite, un ou plusieurs filtres de jointure sont définis en suivant pratiquement la même procédure que celle utilisée pour définir une jointure entre des tables. Les filtres de jointure étendent le filtre de lignes pour que les données des tables associées soient répliquées uniquement si elles correspondent à la clause de filtrage de jointure.  
  
 En règle générale, les filtres de jointure suivent les relations clé primaire/clé étrangère définies pour les tables auxquelles ils sont appliqués, mais ils ne sont pas strictement limités à ces relations. Le filtre de jointure peut être basé sur n'importe quelle logique qui compare des données associées dans deux tables d'articles.  
  
> [!IMPORTANT]  
>  Les filtres de jointure peuvent impliquer un nombre illimité de tables, mais les filtres avec un grand nombre de tables peuvent affecter les performances au cours du traitement de la fusion. Si vous générez des filtres de jointure pour au moins cinq tables, envisagez d'autres solutions : ne filtrez pas les petites tables, les tables qui ne changent pas ou les tables de recherche principales. Utilisez des filtres de jointure uniquement entre les tables qui doivent être partitionnées entre les abonnés.  
  
## <a name="options"></a>Options  
 Cette boîte de dialogue implique un traitement en trois étapes pour créer un filtre de jointure entre deux tables. La création de plusieurs filtres de jointure implique que plusieurs filtres passent par la boîte de dialogue.  
  
1.  **Vérifiez la table filtrée et sélectionnez la table jointe.**  
  
    -   Si vous ajoutez une nouvelle jointure, vérifiez que la table dans la zone de texte **Table filtrée** est correcte (si elle n'est pas correcte, cliquez sur **Annuler**, sélectionnez la table appropriée dans la page **Filtrer les lignes de la table** et sur **Ajouter une jointure** pour revenir à cette boîte de dialogue). Sélectionnez ensuite une table dans la zone de liste déroulante **Table jointe** .  
  
    -   Si vous ajoutez une jointure existante, les noms des tables sont déjà définis et ne peuvent pas être modifiés. Pour modifier les tables impliquées dans la jointure, vous devez supprimer le filtre de jointure existant dans la page **Filtrer les lignes de la table** et créer une nouvelle jointure entre les nouvelles tables.  
  
2.  **Créez l'instruction de jointure.**  
  
    -   Si vous ajoutez une nouvelle jointure, sélectionnez **Utiliser le générateur pour créer l'instruction** ou **Créer manuellement l'instruction de jointure**. Si vous créez une jointure manuellement, vous pouvez utiliser le générateur.  
  
         Si vous sélectionnez le générateur, utilisez les colonnes de la grille (**Conjonction**, **Colonnes de table filtrée**, **Opérateur**et **Colonnes de table jointe**) pour créer une instruction de jointure. Chaque colonne de la grille contient une zone de liste déroulante qui vous permet de sélectionner deux colonnes et un opérateur (**=**, **<>**, **<=**, **\<**, **>=**, **>**, **tel**). Les résultats s'affichent dans la zone de texte **Aperçu** . Si la jointure implique plusieurs paires de colonnes, sélectionnez une conjonction (**AND** ou **OR**) dans la colonne **Conjonction** , puis entrez au moins deux colonnes et un autre opérateur.  
  
         Si vous créez l'instruction manuellement, écrivez l'instruction de jointure dans la zone de texte **Instruction de jointure** . Utilisez la zone de liste **Colonnes de table filtrée** et la zone de liste **Colonnes de table jointe** pour faire glisser les colonnes et les déposer dans la zone de texte **Instruction de jointure** .  
  
    -   Si vous modifiez une jointure existante, vous devez effectuer les modifications manuellement.  
  
3.  **Spécifiez les options de jointure.**  
  
    -   Si la colonne sur laquelle vous effectuez la jointure dans la table filtrée est unique, sélectionnez **Clé unique**. Le processus de fusion possède des fonctionnalités d'optimisation de performances spéciales disponibles si la colonne est unique.  
  
        > [!CAUTION]  
        >  La sélection de cette option indique que la relation entre les tables enfant et parent dans un filtre de jointure correspond à une relation Un à un ou Un à plusieurs. Sélectionnez uniquement cette option s'il existe une contrainte sur la colonne de jointure dans la table parent qui garantit l'unicité. Si vous ne définissez pas correctement l'option, des erreurs de non-convergence de données peuvent se produire.  
  
    -   [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] et versions ultérieures uniquement. Par défaut, la réplication de fusion traite les modifications ligne par ligne lors de la synchronisation. Pour traiter les modifications associées sous la forme d'une unité, sélectionnez **Enregistrement logique**. Cette option est disponible uniquement si les conditions d'article et de publication d'utilisation d'enregistrements logiques sont satisfaites. Pour plus d’informations, consultez la section « Considérations relatives à l’utilisation d’enregistrements logiques » dans [Regrouper les modifications apportées à des lignes connexes à l’aide d’enregistrements logiques](../../relational-databases/replication/merge/group-changes-to-related-rows-with-logical-records.md).  
  
 Après avoir ajouté ou modifié un filtre, cliquez sur **OK** pour enregistrer les modifications et fermer ainsi la boîte de dialogue. Le filtre que vous spécifiez est ensuite analysé et exécuté sur la table indiquée dans la clause SELECT. Si l'instruction de filtrage contient des erreurs de syntaxe ou d'autres erreurs, vous recevrez un message et pourrez modifier l'instruction de filtrage.  
  
## <a name="see-also"></a> Voir aussi  
 [Create a Publication](../../relational-databases/replication/publish/create-a-publication.md)   
 [Afficher et modifier les propriétés d’une publication](../../relational-databases/replication/publish/view-and-modify-publication-properties.md)   
 [Filtrer des données publiées](../../relational-databases/replication/publish/filter-published-data.md)   
 [Join Filters](../../relational-databases/replication/merge/join-filters.md)   
 [Parameterized Row Filters](../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md)   
 [Publier des données et des objets de base de données](../../relational-databases/replication/publish/publish-data-and-database-objects.md)  
  
  
