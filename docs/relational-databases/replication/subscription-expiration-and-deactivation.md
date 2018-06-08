---
title: Expiration et désactivation des abonnements | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: replication
ms.reviewer: ''
ms.suite: sql
ms.technology:
- replication
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- Distributors [SQL Server replication], distribution retention period
- subscriptions [SQL Server replication], expiration
- publications [SQL Server replication], publication retention periods
- expiration [SQL Server replication]
- retention periods [SQL Server replication]
- publication retention periods
- distribution retention period
- subscriptions [SQL Server replication], deactivation
- deactivating subscriptions
ms.assetid: 4d03f5ab-e721-4f56-aebc-60f6a56c1e07
caps.latest.revision: 45
author: MashaMSFT
ms.author: mathoma
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 5135c5773a99804e692d0e46467d5d62dcd0d02c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="subscription-expiration-and-deactivation"></a>Expiration et désactivation des abonnements
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  Les abonnements peuvent expirer ou être désactivés s'ils ne sont pas synchronisés durant une certaine *période de rétention*. L'action qui se produit dépend du type de réplication et de période de rétention qui est dépassée.  
  
 Pour définir des périodes de rétention, consultez [Définir la période d’expiration des abonnements](../../relational-databases/replication/publish/set-the-expiration-period-for-subscriptions.md), [Définir la période de rétention de distribution pour les publications transactionnelles &#40;SQL Server Management Studio&#41;](../../relational-databases/replication/set-distribution-retention-period-for-transactional-publications.md) et [Configurer la publication et la distribution](../../relational-databases/replication/configure-publishing-and-distribution.md).  
  
## <a name="transactional-replication"></a>Réplication transactionnelle  
 La réplication transactionnelle utilise la période de rétention de distribution maximale (le paramètre **@max_distretention** de [sp_adddistributiondb &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-adddistributiondb-transact-sql.md)) et la période de rétention de publication (le paramètre **@retention** de [sp_addpublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpublication-transact-sql.md)) :  
  
-   Si un abonnement n'est pas synchronisé durant la période de rétention maximale (par défaut, 72 heures) et si aucune des modifications intervenues dans la base de données de distribution n'a été remise à l'Abonné, l'abonnement sera marqué comme désactivé par le travail de **nettoyage de distribution** qui s'exécute sur le serveur de distribution. L'abonnement doit être réinitialisé.  
  
-   Si un abonnement n'est pas synchronisé durant la période de rétention de publication (par défaut, 336 heures), l'abonnement expire et est supprimé par le travail de **nettoyage de l'abonnement expiré** qui s'exécute sur le serveur de publication. L'abonnement doit être recréé et synchronisé.  
  
     Si un abonnement par envoi de données expire, il est totalement supprimé, mais ce n'est pas le cas pour les abonnements par extraction de données. Vous devez nettoyer les abonnements par extraction de données au niveau de l'Abonné. Pour plus d’informations, voir [Delete a Pull Subscription](../../relational-databases/replication/delete-a-pull-subscription.md).  
  
## <a name="merge-replication"></a>Réplication de fusion  
 La réplication de fusion utilise la période de rétention de publication (paramètres **@retention** et **@retention_period_unit** de [sp_addmergepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergepublication-transact-sql.md)). Lorsqu'un abonnement expire il doit être réinitialisé, parce que les métadonnées de l'abonnement sont supprimées. Les abonnements qui ne sont pas réinitialisés sont supprimés par le travail de **nettoyage de l'abonnement expiré** sur le serveur de publication. Par défaut, ce travail s'exécute chaque jour ; il supprime tous les abonnements par envoi de données qui ne se sont pas synchronisés au bout de deux fois la période de rétention de publication. Exemple :  
  
-   Si une publication a une période de rétention de 14 jours, un abonnement peut expirer s'il ne s'est pas synchronisé au bout de 14 jours.  
  
     Si le serveur de publication exécute [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] ou version ultérieure et que l'Agent de l'abonnement provient de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] ou version ultérieure, un abonnement n'arrive à expiration que si des modifications de données se sont produites dans la partition de cet abonnement. Supposons par exemple qu'un Abonné ne reçoive de données clients que pour ses clients allemands. Si la période de rétention est définie à 14 jours, l'abonnement n'expire le quatorzième jour que s'il y a eu des modifications dans les données des clients allemands durant les 14 jours écoulés.  
  
-   Entre 14 et 27 jours après la dernière synchronisation, l'abonnement peut être réinitialisé.  
  
-   28 jours après la dernière synchronisation, l'abonnement est supprimé par le travail de **nettoyage de l'abonnement expiré** . Si un abonnement par envoi de données expire, il est totalement supprimé, mais ce n'est pas le cas pour les abonnements par extraction de données. Vous devez nettoyer les abonnements par extraction de données au niveau de l'Abonné. Pour plus d’informations, voir [Delete a Pull Subscription](../../relational-databases/replication/delete-a-pull-subscription.md).  
  
### <a name="considerations-for-setting-the-publication-retention-period-for-merge-publications"></a>Points à prendre en compte pour définir la période de rétention de publication pour les publications de fusion  
 Gardez en mémoire les points suivants lorsque vous définissez la période de rétention de publication pour les publications de fusion :  
  
-   La période de rétention pour les publications de fusion offre un délai de grâce de 24 heures pour tenir compte des Abonnés situés dans différents fuseaux horaires. Si, par exemple, vous définissez une période de rétention d'un jour, la période de rétention réelle est de 48 heures.  
  
-   Le nettoyage des métadonnées des réplications de fusion dépend de la période de rétention de publication :  
  
    -   La réplication ne peut pas nettoyer les métadonnées dans les bases de données de publication et d'abonnement tant que la période de rétention n'est pas achevée. Soyez prudent si vous spécifiez une longue période de rétention, car cela peut affecter négativement les performances de réplication. Vous avez intérêt à spécifier une valeur faible si vous êtes certain que tous les Abonnés procéderont régulièrement à la synchronisation dans ce délai.  
  
    -   Il est possible de spécifier que les abonnements n'expirent jamais (valeur 0 pour **@retention**), mais ceci est fortement déconseillé car les métadonnées ne pourront pas être nettoyées.  
  
-   La période de rétention pour tout serveur de republication doit être égale ou inférieure à la période de rétention définie sur le serveur de publication d'origine. Veillez à utiliser les mêmes valeurs de conservation des publications pour tous les serveurs de publication et leurs partenaires de synchronisation respectifs. L’utilisation de valeurs différentes peut produire une non-convergence. Pour modifier cette valeur, réinitialisez l'Abonné afin d'éviter la non-convergence des données.  
  
-   Après un nettoyage, si la période de conservation des publications est augmentée et qu’un abonnement essaie de fusionner avec le serveur de publication (qui a déjà supprimé les métadonnées), l’abonnement n’expire pas en raison de l’augmentation de la valeur de rétention. Cependant, le serveur de publication ne dispose pas d'une quantité suffisante de métadonnées pour télécharger les modifications apportées à l'Abonné, ce qui génère une non-convergence.  
  
## <a name="see-also"></a> Voir aussi  
 [Réinitialiser des abonnements](../../relational-databases/replication/reinitialize-subscriptions.md)   
 [Administration de l’Agent de réplication](../../relational-databases/replication/agents/replication-agent-administration.md)   
 [S'abonner à des publications](../../relational-databases/replication/subscribe-to-publications.md)  
  
  
