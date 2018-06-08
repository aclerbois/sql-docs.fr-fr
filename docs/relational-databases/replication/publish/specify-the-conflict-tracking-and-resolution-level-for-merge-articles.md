---
title: Spécifier le niveau de suivi et de résolution des conflits pour des articles de fusion | Microsoft Docs
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
helpviewer_keywords:
- merge replication conflict resolution [SQL Server replication], levels
- articles [SQL Server replication], conflict resolution
- conflict resolution [SQL Server replication], merge replication
ms.assetid: 81e9ecb6-1d31-4a78-b32a-96f7f4d67077
caps.latest.revision: 35
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 2f3fb598a48665aecf43ea2d3498020229c5767b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="specify-the-conflict-tracking-and-resolution-level-for-merge-articles"></a>Spécifier le niveau de suivi et de résolution des conflits pour des articles de fusion
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Cette rubrique explique comment spécifier le niveau de suivi et de résolution des conflits pour les articles de fusion dans [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../../includes/tsql-md.md)].  
  
 Lorsqu'un abonnement à une publication de fusion est synchronisé, la réplication vérifie la présence de conflits faisant suite à de la modification des mêmes données au niveau du serveur de publication et de l'Abonné. Vous pouvez spécifier si les conflits sont détectés au niveau de la ligne, auquel cas toute modification apportée à la ligne est considérée comme un conflit, ou au niveau de la colonne, auquel cas seules les modifications apportées aux mêmes ligne et colonne sont considérées comme un conflit. La résolution des conflits pour les articles est réalisée au niveau de la ligne. Pour plus d'informations sur la détection et la résolution des conflits avec des enregistrements logiques, consultez [Detecting and Resolving Conflicts in Logical Records](../../../relational-databases/replication/merge/advanced-merge-replication-conflict-resolving-in-logical-record.md).  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Limitations et restrictions](#Restrictions)  
  
-   **Pour spécifier le niveau de suivi et de résolution des conflits pour les article de fusion à l'aide de :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="Restrictions"></a> Limitations et restrictions  
  
-   Si vous modifiez le niveau de suivi une fois les abonnements initialisés, ces derniers doivent être réinitialisés. Pour plus d’informations sur les effets des modifications de propriétés, consultez [Changer des propriétés de publication et d’article](../../../relational-databases/replication/publish/change-publication-and-article-properties.md).  
  
-   Avec le suivi au niveau des lignes et des colonnes, la résolution des conflits est toujours effectuée au niveau des lignes : la ligne gagnante remplace la ligne perdante. La réplication de fusion vous permet également de spécifier que les conflits sont suivis et résolus au niveau des enregistrements logiques, mais ces options ne sont pas disponibles à partir de [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]. Pour plus d'informations sur la définition de ces options à partir des procédures stockées de réplication, consultez [Définir une relation d’enregistrement logique entre des articles de table de fusion](../../../relational-databases/replication/publish/define-a-logical-record-relationship-between-merge-table-articles.md).  
  
##  <a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
 Spécifiez le suivi au niveau des lignes ou des colonnes pour les articles de fusion sous l’onglet **Propriétés** de la boîte de dialogue **Propriétés de l’article**, disponible dans l’Assistant Nouvelle publication et la boîte de dialogue **Propriétés de la publication - \<Publication>**. Pour plus d’informations sur l’utilisation de l’Assistant et sur l’accès à la boîte de dialogue, consultez [Créer une publication](../../../relational-databases/replication/publish/create-a-publication.md) et [Afficher et modifier les propriétés d’une publication](../../../relational-databases/replication/publish/view-and-modify-publication-properties.md).  
  
#### <a name="to-specify-row--or-column-level-tracking"></a>Pour spécifier le suivi au niveau des lignes ou des colonnes  
  
1.  Dans la page **Articles** de l’Assistant Nouvelle publication ou la boîte de dialogue **Propriétés de la publication - \<Publication>**, sélectionnez une table.  
  
2.  Cliquez sur **Propriétés de l'article**, puis cliquez sur **Définir les propriétés de l'article de Table en surbrillance** ou sur **Définir les propriétés de tous les articles de table**.  
  
3.  Sous l’onglet **Propriétés** de la boîte de dialogue **Propriétés de l’article - \<Article>**, sélectionnez l’une des valeurs suivantes pour la propriété **Niveau de suivi** : **Suivi au niveau des lignes** ou **Suivi au niveau des colonnes**.  
  
4.  Si vous êtes dans la boîte de dialogue **Propriétés de la publication - \<Publication>**, cliquez sur **OK** pour enregistrer et fermer la boîte de dialogue.  
  
##  <a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-specify-conflict-tracking-options-for-a-new-merge-article"></a>Pour spécifier les options de suivi des conflits pour un nouvel article de fusion  
  
1.  Dans la base de données de publication sur le serveur de publication, exécutez [sp_addmergearticle](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md) et spécifiez l'une des valeurs suivantes pour **@column_tracking**:  
  
    -   **true** - utiliser le suivi au niveau des colonnes pour l'article.  
  
    -   **false** - utiliser le suivi au niveau des lignes, qui est la valeur par défaut.  
  
#### <a name="to-change-conflict-tracking-options-for-a-merge-article"></a>Pour modifier les options de suivi des conflits pour un article de fusion  
  
1.  Pour déterminer les options de suivi des conflits pour un article de fusion, exécutez [sp_helpmergearticle](../../../relational-databases/system-stored-procedures/sp-helpmergearticle-transact-sql.md). Notez la valeur de l'option **column_tracking** dans le jeu de résultats de l'article. La valeur **1** indique que le suivi au niveau des colonnes est utilisé, tandis que la valeur **0** indique que le suivi au niveau des lignes est utilisé.  
  
2.  Dans la base de données de publication sur le serveur de publication, exécutez [sp_changemergearticle](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md). Affectez la valeur **column_tracking** à **@property** et l'une des valeurs suivantes à **@value**:  
  
    -   **true** - utiliser le suivi au niveau des colonnes pour l'article.  
  
    -   **false** - utiliser le suivi au niveau des lignes, qui est la valeur par défaut.  
  
     Affectez la valeur **1** à **@force_invalidate_snapshot** et **@force_reinit_subscription**.  
  
## <a name="see-also"></a> Voir aussi  
 [Advanced Merge Replication Conflict Detection and Resolution](../../../relational-databases/replication/merge/advanced-merge-replication-conflict-detection-and-resolution.md)   
 [Détection et résolution des conflits dans les enregistrements logiques](../../../relational-databases/replication/merge/advanced-merge-replication-conflict-resolving-in-logical-record.md)   
 [Définir une relation d’enregistrement logique entre des articles de table de fusion](../../../relational-databases/replication/publish/define-a-logical-record-relationship-between-merge-table-articles.md)   
 [Détecter et résoudre des conflits de réplication de fusion](../../../relational-databases/replication/merge/advanced-merge-replication-resolve-merge-replication-conflicts.md)  
  
  
