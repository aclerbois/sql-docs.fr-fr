---
title: Afficher les informations relatives aux conflits pour les publications de fusion | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: replication
ms.reviewer: ''
ms.suite: sql
ms.technology:
- replication
ms.tgt_pltfrm: ''
ms.topic: conceptual
dev_langs:
- TSQL
helpviewer_keywords:
- merge replication conflict resolution [SQL Server replication], viewing conflicts
- sp_helpmergeconflictrows
- viewing conflict information
- conflict resolution [SQL Server replication], merge replication
- sp_helpmergearticleconflicts
ms.assetid: 4907fe35-10ee-4f81-b924-fc419b1864d2
caps.latest.revision: 22
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: a87f0391580dd8f577abf94997fd1448ae5bf198
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="view-conflict-information-for-merge-publications"></a>Afficher les informations relatives aux conflits pour les publications de fusion
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Lorsqu'un conflit est résolu dans une réplication de fusion, les données de la ligne perdante sont écrites dans une table de conflits. Ces données peuvent être affichées par programme en utilisant des procédures stockées de réplication. Pour plus d’informations, consultez [Advanced Merge Replication Conflict Detection and Resolution](../../relational-databases/replication/merge/advanced-merge-replication-conflict-detection-and-resolution.md).  
  
### <a name="to-view-conflict-information-and-losing-row-data-for-all-types-of-conflicts"></a>Pour afficher les informations relatives au conflit et les données de ligne perdante pour tous les types de conflits  
  
1.  Dans la base de données de publication sur le serveur de publication, exécutez [sp_helpmergepublication](../../relational-databases/system-stored-procedures/sp-helpmergepublication-transact-sql.md). Notez les valeurs des colonnes suivantes dans le jeu de résultats :  
  
    -   **centralized_conflicts** - 1 indique que les lignes présentant des conflits sont stockées sur le serveur de publication, et 0 indique que les lignes présentant des conflits ne sont pas stockées sur le serveur de publication.  
  
    -   **decentralized_conflicts** - 1 indique que les lignes présentant des conflits sont stockées sur l'Abonné, et 0 indique que les lignes présentant des conflits ne sont pas stockées sur l'Abonné.  
  
        > [!NOTE]  
        >  Le comportement de journalisation des conflits d'une publication de fusion est défini à l'aide du paramètre **@conflict_logging** de [sp_addmergepublication](../../relational-databases/system-stored-procedures/sp-addmergepublication-transact-sql.md). L'utilisation du paramètre **@centralized_conflicts** est déconseillée.  
  
     Le tableau suivant décrit les valeurs de ces colonnes en fonction de la valeur spécifiée pour **@conflict_logging**.  
  
    |Valeur @conflict_logging|centralized_conflicts|decentralized_conflicts|  
    |------------------------------|----------------------------|------------------------------|  
    |**publisher** (serveur de publication)| 1|0|  
    |**subscriber** (Abonné)|0| 1|  
    |**both** (les deux)| 1| 1|  
  
2.  Dans la base de données de publication sur le serveur de publication ou dans la base de données d'abonnement de l'Abonné, exécutez [sp_helpmergearticleconflicts](../../relational-databases/system-stored-procedures/sp-helpmergearticleconflicts-transact-sql.md). Affectez une valeur à **@publication** afin de retourner uniquement des informations sur les conflits pour les articles qui appartiennent à une publication spécifique. Des informations sur les tables de conflits sont alors retournées pour les articles présentant des conflits. Notez la valeur de **conflict_table** pour tout article qui vous intéresse. Si la valeur de **conflict_table** d'un article est NULL, seuls des conflits de suppression se sont produits dans cet article.  
  
3.  (Facultatif) Examinez les lignes présentant des conflits pour les articles qui vous intéressent. Selon la valeur de **centralized_conflicts** et **decentralized_conflicts** obtenues à l'étape 1, effectuez l'une des opérations suivantes :  
  
    -   Dans la base de données de publication sur le serveur de publication, exécutez [sp_helpmergeconflictrows](../../relational-databases/system-stored-procedures/sp-helpmergeconflictrows-transact-sql.md). Spécifiez une table de conflits pour l'article (obtenue à l'étape 1) pour **@conflict_table**. (Facultatif) Spécifiez une valeur pour **@publication** afin de limiter à une publication spécifique les informations sur les conflits retournées. Des données de ligne et autres informations sur la ligne perdante sont alors retournées.  
  
    -   Dans la base de données d'abonnement de l'Abonné, exécutez [sp_helpmergeconflictrows](../../relational-databases/system-stored-procedures/sp-helpmergeconflictrows-transact-sql.md). Spécifiez une table de conflits pour l'article (obtenue à l'étape 1) pour **@conflict_table**. Des données de ligne et autres informations sur la ligne perdante sont alors retournées.  
  
### <a name="to-view-information-only-on-conflicts-where-the-delete-failed"></a>Pour afficher des informations uniquement sur les conflits avec échec de suppression  
  
1.  Dans la base de données de publication sur le serveur de publication, exécutez [sp_helpmergepublication](../../relational-databases/system-stored-procedures/sp-helpmergepublication-transact-sql.md). Notez les valeurs des colonnes suivantes dans le jeu de résultats :  
  
    -   **centralized_conflicts** - 1 indique que les lignes présentant des conflits sont stockées sur le serveur de publication, et 0 indique que les lignes présentant des conflits ne sont pas stockées sur le serveur de publication.  
  
    -   **decentralized_conflicts** - 1 indique que les lignes présentant des conflits sont stockées sur l'Abonné, et 0 indique que les lignes présentant des conflits ne sont pas stockées sur l'Abonné.  
  
        > [!NOTE]  
        >  Le comportement de journalisation des conflits d'une publication de fusion est défini à l'aide du paramètre **@conflict_logging** de [sp_addmergepublication](../../relational-databases/system-stored-procedures/sp-addmergepublication-transact-sql.md). L'utilisation du paramètre **@centralized_conflicts** est déconseillée.  
  
2.  Dans la base de données de publication sur le serveur de publication ou dans la base de données d'abonnement de l'Abonné, exécutez [sp_helpmergearticleconflicts](../../relational-databases/system-stored-procedures/sp-helpmergearticleconflicts-transact-sql.md). Affectez une valeur à **@publication** afin de retourner uniquement des informations sur les tables de conflits pour les articles qui appartiennent à une publication spécifique. Des informations sur les tables de conflits sont alors retournées pour les articles présentant des conflits. Notez la valeur de **source_object** pour tout article qui vous intéresse. Si la valeur de **conflict_table** d'un article est NULL, seuls des conflits de suppression se sont produits dans cet article.  
  
3.  (Facultatif) Examinez les informations sur les conflits pour les conflits de suppression. Selon la valeur de **centralized_conflicts** et **decentralized_conflicts** obtenues à l'étape 1, effectuez l'une des opérations suivantes :  
  
    -   Dans la base de données de publication sur le serveur de publication, exécutez [sp_helpmergedeleteconflictrows](../../relational-databases/system-stored-procedures/sp-helpmergedeleteconflictrows-transact-sql.md). Spécifiez le nom de la table source (obtenue à l'étape 1) sur laquelle le conflit s'est produit pour **@source_object**. (Facultatif) Spécifiez une valeur pour **@publication** afin de limiter à une publication spécifique les informations sur les conflits retournées. Les informations sur les conflits de suppression stockées sur le serveur de publication sont alors retournées.  
  
    -   Dans la base de données d'abonnement de l'Abonné, exécutez [sp_helpmergedeleteconflictrows](../../relational-databases/system-stored-procedures/sp-helpmergedeleteconflictrows-transact-sql.md). Spécifiez le nom de la table source (obtenue à l'étape 1) sur laquelle le conflit s'est produit pour **@source_object**. (Facultatif) Spécifiez une valeur pour **@publication** afin de limiter à une publication spécifique les informations sur les conflits retournées. Les informations sur les conflits de suppression stockées sur l'Abonné sont alors retournées.  
  
## <a name="see-also"></a> Voir aussi  
 [Détection et résolution des conflits de réplication de fusion avancée](../../relational-databases/replication/merge/advanced-merge-replication-conflict-detection-and-resolution.md)  
  
  
