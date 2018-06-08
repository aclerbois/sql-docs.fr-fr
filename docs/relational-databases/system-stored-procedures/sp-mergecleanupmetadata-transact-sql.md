---
title: sp_mergecleanupmetadata (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology:
- replication
ms.tgt_pltfrm: ''
ms.topic: language-reference
applies_to:
- SQL Server
f1_keywords:
- sp_mergecleanupmetadata_TSQL
- sp_mergecleanupmetadata
helpviewer_keywords:
- sp_mergecleanupmetadata
ms.assetid: 892f8628-4cbe-4cc3-b959-ed45ffc24064
caps.latest.revision: 17
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 026675528003028ffd3fd73301f47f75bcab0575
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="spmergecleanupmetadata-transact-sql"></a>sp_mergecleanupmetadata (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Doit être utilisé uniquement dans les topologies de réplication qui incluent des serveurs exécutant des versions de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] antérieures à [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] Service Pack 1. **sp_mergecleanupmetadata** permet aux administrateurs de nettoyer les métadonnées dans le **MSmerge_genhistory**, **MSmerge_contents** et **MSmerge_tombstone** tables système. Cette procédure stockée est exécutée sur le serveur de publication dans la base de données de publication.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_mergecleanupmetadata [ [ @publication = ] 'publication' ]  
    [ , [ @reinitialize_subscriber = ] 'reinitialize_subscriber' ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@publication =** ] **'***publication***'**  
 Nom de la publication. *publication* est **sysname**, avec une valeur par défaut **%**, qui nettoie les métadonnées pour toutes les publications. La publication doit déjà exister si elle est spécifiée de manière explicite.  
  
 [  **@reinitialize_subscriber =** ] **'***abonné***'**  
 Spécifie si réinitialiser l'abonné. *abonné* est **nvarchar (5)**, peut être **TRUE** ou **FALSE**, avec une valeur par défaut **TRUE**. Si **TRUE**, les abonnements sont marqués pour réinitialisation. Si **FALSE**, les abonnements ne sont pas marqués pour réinitialisation.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_mergecleanupmetadata** doit être utilisé uniquement dans les topologies de réplication qui incluent des serveurs exécutant des versions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] antérieures à [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] Service Pack 1. Les topologies qui incluent uniquement [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] Service Pack 1 ou une version ultérieure doivent utiliser la rétention automatique basée sur le nettoyage des métadonnées. Lorsque vous exécutez cette procédure stockée, soyez conscient de l'importance de la croissance nécessaire et potentielle du fichier journal sur l'ordinateur sur lequel la procédure stockée est exécutée.  
  
> [!CAUTION]  
>  Après avoir **sp_mergecleanupmetadata** est exécutée, par défaut, tous les abonnements aux abonnés de publications qui ont des métadonnées stockées dans **MSmerge_genhistory**, **MSmerge_contents** et **MSmerge_tombstone** sont marqués pour réinitialisation, les modifications en attente sur l’abonné sont perdues, et l’instantané actuel est marqué comme obsolète.  
  
> [!NOTE]  
>  S’il existe plusieurs publications sur une base de données, et l’une des publications utilise une période de rétention de publication infinie (**@retention**=**0**), il est en cours d’exécution **sp_mergecleanupmetadata** ne nettoie pas les métadonnées pour la base de données de suivi de la modification de réplication de fusion. C'est pour cette raison qu'il faut utiliser la période de rétention infinie avec prudence.  
  
 Lors de l’exécution de cette procédure, vous pouvez choisir s’il faut réinitialiser les abonnés en définissant le **@reinitialize_subscriber** paramètre **TRUE** (la valeur par défaut) ou **FALSE**. Si **sp_mergecleanupmetadata** est exécutée avec le **@reinitialize_subscriber** paramètre la valeur **TRUE**, un instantané est réappliqué sur l’abonné même si l’abonnement a été créé sans un instantané (par exemple, si les données d’instantané et le schéma ont été appliqués manuellement ou existe déjà sur l’abonné) initial. Le paramètre **FALSE** doit être utilisée avec précaution, car la publication n’est pas réinitialisée, vous devez vous assurer que les données sur le serveur de publication et l’abonné sont synchronisées.  
  
 Quelle que soit la valeur de **@reinitialize_subscriber**, **sp_mergecleanupmetadata** échoue s’il n’y en cours de fusion des processus qui tentent de télécharger les modifications vers un serveur de publication ou un abonné de republication au moment de la procédure stockée est appelée.  
  
 **Exécution de sp_mergecleanupmetadata avec @reinitialize_subscriber = TRUE :**  
  
1.  Bien qu'il ne soit pas nécessaire d'arrêter toutes les mises à jour des bases de données d'abonnement et de publication, cette opération est recommandée. Si les mises à jour se poursuivent, toute mise à jour réalisée sur un abonné depuis la dernière fusion est perdue lorsque la publication est réinitialisée, mais les données de convergence sont conservées.  
  
2.  Exécutez une fusion à l'aide de l'Agent de fusion. Nous vous recommandons d’utiliser le **– Validate** option de ligne de commande de l’agent sur chaque abonné lors de l’exécution de l’Agent de fusion. Si vous exécutez des fusions en mode continu, consultez *Remarques importantes concernant les fusions en Mode continu* plus loin dans cette section.  
  
3.  Une fois toutes les fusions ont terminé, exécutez **sp_mergecleanupmetadata**.  
  
4.  Exécutez **sp_reinitmergepullsubscription** sur tous les abonnés utilisant un abonnement par extraction nommé ou anonyme pour garantir la convergence des données.  
  
5.  Si vous exécutez des fusions en mode continu, consultez *Remarques importantes concernant les fusions en Mode continu* plus loin dans cette section.  
  
6.  Régénérez les fichiers d'instantané de toutes les publications de fusion impliquées à tous les niveaux. Si vous tentez d'effectuer la fusion sans régénérer l'instantané au préalable, vous recevez un message vous invitant à la régénérer.  
  
7.  Sauvegardez la base de données de publication. car sans cela, une opération de fusion peut échouer après la restauration de la base de données de publication.  
  
 **Exécution de sp_mergecleanupmetadata avec @reinitialize_subscriber = FALSE :**  
  
1.  Arrêter **tous les** mises à jour pour les bases de données de publication et d’abonnement.  
  
2.  Exécutez une fusion à l'aide de l'Agent de fusion. Nous vous recommandons d’utiliser le **– Validate** option de ligne de commande de l’agent sur chaque abonné lors de l’exécution de l’Agent de fusion. Si vous exécutez des fusions en mode continu, consultez *Remarques importantes concernant les fusions en Mode continu* plus loin dans cette section.  
  
3.  Une fois toutes les fusions ont terminé, exécutez **sp_mergecleanupmetadata**.  
  
4.  Si vous exécutez des fusions en mode continu, consultez *Remarques importantes concernant les fusions en Mode continu* plus loin dans cette section.  
  
5.  Régénérez les fichiers d'instantané de toutes les publications de fusion impliquées à tous les niveaux. Si vous tentez d'effectuer la fusion sans régénérer l'instantané au préalable, vous recevez un message vous invitant à la régénérer.  
  
6.  Sauvegardez la base de données de publication. car sans cela, une opération de fusion peut échouer après la restauration de la base de données de publication.  
  
 **Remarques importantes concernant les fusions en Mode continu**  
  
 Si vous exécutez des fusions en mode continu, vous devez :  
  
-   Arrêtez l’Agent de fusion, puis effectuer une autre fusion sans le **-continue** paramètre spécifié.  
  
-   Désactiver la publication avec **sp_changemergepublication** pour vous assurer que toutes les fusions en mode continu sollicitant l’état de la publication.  
  
    ```  
    EXEC central..sp_changemergepublication @publication = 'dynpart_pubn', @property = 'status', @value = 'inactive'  
    ```  
  
 Lorsque vous avez terminé l’étape 3 de l’exécution **sp_mergecleanupmetadata**, reprenez les fusions en mode continu en fonction de la façon dont vous les avez arrêtées. Vous pouvez :  
  
-   Ajouter le **– continu** paramètre renvoyé par l’Agent de fusion.  
  
-   Réactiver la publication avec **sp_changemergepublication.**  
  
    ```  
    EXEC central..sp_changemergepublication @publication = 'dynpart_pubn', @property = 'status', @value = 'active'  
    ```  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres de la **sysadmin** rôle serveur fixe ou **db_owner** du rôle de base de données fixe peut exécuter **sp_mergecleanupmetadata**.  
  
 Pour utiliser cette procédure stockée, le serveur de publication doit exécuter [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)], Les abonnés doivent exécuter [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] ou [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 7.0, Service Pack 2.  
  
## <a name="see-also"></a>Voir aussi  
 [MSmerge_genhistory &#40;Transact-SQL&#41;](../../relational-databases/system-tables/msmerge-genhistory-transact-sql.md)   
 [MSmerge_contents &#40;Transact-SQL&#41;](../../relational-databases/system-tables/msmerge-contents-transact-sql.md)   
 [MSmerge_tombstone &#40;Transact-SQL&#41;](../../relational-databases/system-tables/msmerge-tombstone-transact-sql.md)  
  
  
