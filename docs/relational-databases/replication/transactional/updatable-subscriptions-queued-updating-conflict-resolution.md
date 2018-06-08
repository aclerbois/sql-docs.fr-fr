---
title: Détection et résolution des conflits de mise à jour en attente | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
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
- conflict resolution [SQL Server replication], queued updating subscriptions
- viewing queued updating conflicts
- conflict resolution [SQL Server replication]
- queued updating subscriptions [SQL Server replication]
- articles [SQL Server replication], conflict resolution
ms.assetid: 084ac587-25e7-4bd0-a385-556bbe07d02f
caps.latest.revision: 39
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 813816639e32b92d9447547e7fcb53b8db5e8fee
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="updatable-subscriptions---queued-updating-conflict-resolution"></a>Abonnements pouvant être mis à jour - Résolution des conflits de mise à jour en attente
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Étant donné que les abonnements mis à jour en attente permettent d'apporter des modifications aux mêmes données à différents emplacements, des conflits sont susceptibles de se produire lors de la synchronisation des données sur le serveur de publication. La réplication détecte les éventuels conflits lors de la synchronisation des modifications avec le serveur de publication et les résout à l'aide de la stratégie de résolution que vous avez sélectionnée lors de la création de la publication. Les conflits suivants peuvent se produire :  
  
-   Conflits de mise à jour et d'insertion. Ce type de conflit survient lorsque les mêmes données sont modifiées dans deux endroits. L'un « gagne », l'autre « perd ».  
  
-   Conflits de suppression. Ce type de conflits survient lorsque la même ligne est supprimée à un endroit et modifiée à un autre.  
  
 La détection et la résolution des conflits pouvant s'avérer gourmandes en temps et en ressources, il est préférable d'éviter au maximum les conflits dans l'application en créant des partitions de données pour que les différents Abonnés modifient des sous-ensembles de données différents.  
  
## <a name="detecting-conflicts"></a>Détection des conflits  
 Lorsque vous créez une publication et activez les mises à jour en attente, la réplication ajoute à la table sous-jacente une colonne de type **uniqueidentifier** (**msrepl_tran_version**) ayant comme valeur par défaut **newid()** . Lorsque les données publiées sont modifiées sur l'éditeur ou sur l'abonné, la ligne reçoit un nouvel identificateur global unique (GUID) indiquant qu'il s'agit d'une nouvelle version. L'Agent de lecture de la file d'attente utilise cette colonne au cours de la synchronisation pour déterminer si un conflit existe.  
  
 Une transaction en file d'attente conserve les valeurs de l'ancienne et de la nouvelle version de ligne. Lorsque la transaction est appliquée sur l'éditeur, le GUID provenant de la transaction et celui contenu dans la publication sont comparés. Si l'ancien GUID stocké dans la transaction correspond à celui contenu dans la publication, la publication est mise à jour et la ligne reçoit le nouveau GUID généré par l'abonné. La mise à jour de la publication avec le GUID issu de la transaction permet d'obtenir des versions de lignes identiques dans la publication et dans la transaction.  
  
 Si l'ancien GUID stocké dans la transaction ne correspond pas au GUID contenu dans la publication, un conflit est détecté. Le nouveau GUID contenu dans la publication indique l'existence de deux versions de ligne : l'une dans la transaction que soumet l'abonné et l'autre, plus récente, sur le serveur de publication. Cela signifie qu'un autre abonné ou l'éditeur a mis à jour la même ligne dans la publication avant que la transaction soumise par l'abonné ne soit synchronisée.  
  
 À la différence de la réplication de fusion, la colonne GUID n'est pas utilisée pour identifier la ligne proprement dite, mais pour vérifier si elle a changé.  
  
## <a name="resolving-conflicts"></a>Résolution de conflits  
 Lorsque vous créez une publication acceptant les mises à jour en attente, vous sélectionnez un programme de résolution de conflits qui sera utilisé en cas de détection de conflit. Le programme de résolution de conflits indique à l'Agent de lecture de la file d'attente comment il doit traiter les différentes versions d'une même ligne rencontrées au cours de la synchronisation. Vous pouvez modifier la stratégie de résolution de conflit après avoir créé la publication tant qu'aucun abonnement n'est associé à celle-ci. Le programme de résolution de conflits propose les choix suivants :  
  
-   Le serveur de publication gagne (valeur par défaut)  
  
-   L'éditeur gagne et l'abonnement est réinitialisé  
  
-   L'abonné gagne  
  
 Les conflits sont enregistrés et peuvent être visualisés à l'aide de l'outil d'affichage des conflits.  
  
 **Pour définir la stratégie de résolution des conflits de mise à jour en attente**  
  
-   [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] : [Définir des options de résolution des conflits de mise à jour en attente &#40;SQL Server Management Studio&#41;](../../../relational-databases/replication/publish/set-queued-updating-conflict-resolution-options-sql-server-management-studio.md)  
  
-   Programmation Transact-SQL de la réplication : [Activer les abonnements pouvant être mis à jour pour les publications transactionnelles](../../../relational-databases/replication/publish/enable-updating-subscriptions-for-transactional-publications.md)  
  
 **Pour afficher les conflits de données**  
  
-   [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] : [Afficher les conflits de données pour les publications transactionnelles &#40;SQL Server Management Studio&#41;](../../../relational-databases/replication/view-data-conflicts-for-transactional-publications-sql-server-management-studio.md)  
  
### <a name="publisher-wins"></a>L'éditeur gagne  
 Lorsque la résolution de conflit appliquée est « Le serveur de publication gagne », la cohérence transactionnelle est conservée dans les données situées sur le serveur de publication. La transaction en conflit est annulée sur l'abonné qui l'a initialisée.  
  
 L'Agent de lecture de la file d'attente détecte un conflit, puis des commandes de compensation sont générées et propagées vers l'abonné par l'intermédiaire de la base de données de distribution. L'Agent de distribution applique ensuite les commandes de compensation à l'abonné responsable de la transaction conflictuelle. Les opérations de compensation mettent à jour les lignes sur l'Abonné afin qu'elles correspondent à celles du serveur de publication.  
  
 Avant que les commandes de compensation ne soient appliquées, il est possible de lire les résultats d'une transaction qui sera finalement annulée sur l'Abonné. Cela équivaut à une lecture incorrecte (niveau d'isolation lecture non validée). Il n'y a pas de compensation pour les transactions suivantes qui peuvent en découler. Toutefois, les limites des transactions sont respectées et les actions comprises dans une transaction sont soit toutes validées, soit (en cas de conflit) toutes annulées.  
  
### <a name="publisher-wins-and-the-subscription-is-reinitialized"></a>L'éditeur gagne et l'abonnement est réinitialisé  
 La réinitialisation de l'Abonné pour résoudre les conflits permet de conserver une cohérence transactionnelle stricte sur l'Abonné mais peut prendre beaucoup de temps si la publication contient d'importants volumes de données.  
  
 Lorsque l'Agent de lecture de la file d'attente détecte un conflit, toutes les transactions qui figurent dans la file d'attente (y compris la transaction en conflit) sont rejetées et l'abonné est marqué pour la réinitialisation. L'instantané suivant généré pour la publication est appliqué à l'Abonné par l'Agent de distribution.  
  
### <a name="subscriber-wins"></a>L'abonné gagne  
 La détection de conflit d'après la stratégie " L'abonné gagne " signifie que la dernière transaction de l'abonné destinée à mettre à jour l'éditeur l'emporte. Dans ce cas, en cas de détection de conflit, la transaction envoyée par l'abonné est utilisée et l'éditeur est mis à jour. Cette stratégie s'adresse aux applications où de telles modifications ne compromettent pas l'intégrité des données.  
  
## <a name="see-also"></a> Voir aussi  
 [Updatable Subscriptions for Transactional Replication](../../../relational-databases/replication/transactional/updatable-subscriptions-for-transactional-replication.md)  
  
  
