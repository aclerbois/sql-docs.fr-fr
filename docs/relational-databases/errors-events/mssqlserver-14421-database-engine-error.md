---
title: MSSQLSERVER_14421 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: supportability
ms.tgt_pltfrm: ''
ms.topic: language-reference
helpviewer_keywords:
- 14421 (Database Engine error)
ms.assetid: 03e76d4a-d463-4673-8843-08e4ecaefe27
caps.latest.revision: 23
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 6916ede3c26b066df82ad53c681633e7d8777d7a
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="mssqlserver14421"></a>MSSQLSERVER_14421
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  
## <a name="details"></a>Détails  
  
|||  
|-|-|  
|Nom du produit|SQL Server|  
|ID d'événement|14421|  
|Source de l'événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|SQLErrorNum14421|  
|Texte du message|La base de données secondaire d'envoi des journaux %s.%s a dépassé le seuil de restauration de %d minutes et n'est pas synchronisée. Aucune restauration n'a été effectuée depuis %d minutes. La latence de la restauration est de %d minutes. Vérifiez les informations du journal de l'agent et du moniteur de copie des journaux de transaction.|  
  
## <a name="explanation"></a>Explication  
Ce message indique que la copie des journaux de transaction n'est plus synchronisée en cas de dépassement du seuil de restauration. Le seuil de restauration correspond au nombre de minutes qui peuvent s'écouler entre les opérations de restauration avant qu'un message soit généré.  
  
### <a name="possible-causes"></a>Causes possibles  
Ce message n'indique pas nécessairement un problème avec la copie des journaux de transaction. Il peut, en fait, indiquer l'un des problèmes suivants :  
  
-   Le travail de restauration n'est pas en cours d'exécution.  
  
    Le travail peut ne pas s'exécuter pour les raisons suivantes : le service Agent SQL Server sur l'instance du serveur secondaire n'est pas en cours d'exécution, le travail est désactivé ou la planification du travail a été modifiée.  
  
-   Le travail de restauration est en échec.  
  
    Le travail peut échouer pour les raisons suivantes : le chemin d'accès du dossier de restauration n'est pas valide, le disque est plein ou toute autre raison pouvant entraîner l'échec de l'instruction RESTORE.  
  
## <a name="user-action"></a>Action de l'utilisateur  
Pour résoudre le problème qui est à l'origine de ce message :  
  
-   Assurez-vous que le service Agent SQL Server est en cours d'exécution pour l'instance du serveur secondaire et que le travail de restauration pour cette base de données secondaire est activé et est planifié pour s'exécuter à la fréquence appropriée.  
  
-   Il est possible que le travail de restauration sur le serveur secondaire soit en échec. Dans ce cas, vérifiez l'historique des travaux de restauration pour déterminer la cause de l'échec.  
  
-   Il se peut que le travail de restauration de la copie des journaux de transaction, qui s’exécute sur l’instance du serveur secondaire, ne puisse pas se connecter à l’instance du serveur moniteur pour mettre à jour la table **log_shipping_monitor_secondary**. Ceci peut être dû à un problème d'authentification entre l'instance du serveur moniteur et l'instance du serveur secondaire.  
  
-   La valeur du seuil d'alerte de sauvegarde est peut-être incorrecte. Dans l'idéal, cette valeur doit être au moins trois fois supérieure à la fréquence du travail de restauration. Si vous modifiez la fréquence du travail de restauration une fois que la copie des journaux de transaction est configurée et fonctionnelle, la valeur du seuil d'alerte de sauvegarde doit être mise à jour en conséquence.  
  
-   Lorsque l’instance du serveur moniteur est mise hors connexion et qu’elle revient ensuite en ligne, la table **log_shipping_monitor_secondary** n’est pas mise à jour avec les valeurs actuelles avant l’exécution du travail de message d’alerte. L'erreur 14421 peut survenir lorsqu'un travail de restauration est effectué et retourne le message « Impossible de trouver un fichier de sauvegarde du journal pouvant être appliqué à la base de données secondaire ». Dans ce cas, l'heure de restauration n'est pas mise à jour. La cause de l'erreur peut alors être liée à un problème du travail de copie.  
  
    Pour mettre à jour les tables du serveur moniteur afin qu’elles contiennent les données les plus récentes pour la base de données secondaire, exécutez **sp_refresh_log_shipping_monitor** sur l’instance du serveur secondaire.  
  
-   La date ou l'heure est incorrecte sur l'instance du serveur secondaire ou du serveur moniteur. Ceci peut également générer des messages d'alerte. Il est possible que la date ou l'heure système ait été modifiée sur l'une des deux instances.  
  
    > [!NOTE]  
    > Une différence de fuseaux horaires entre les deux instances de serveurs ne pose généralement aucun problème.  
  
## <a name="see-also"></a> Voir aussi  
[log_shipping_monitor_secondary &#40;Transact-SQL&#41;](~/relational-databases/system-tables/log-shipping-monitor-secondary-transact-sql.md)  
[À propos de la copie des journaux de transaction &#40;SQL Server&#41;](~/database-engine/log-shipping/about-log-shipping-sql-server.md)  
[sp_help_log_shipping_monitor_secondary &#40;Transact-SQL&#41;](~/relational-databases/system-stored-procedures/sp-help-log-shipping-monitor-secondary-transact-sql.md)  
[sp_refresh_log_shipping_monitor &#40;Transact-SQL&#41;](~/relational-databases/system-stored-procedures/sp-help-log-shipping-monitor-transact-sql.md)  
[À propos de la copie des journaux de transaction &#40;SQL Server&#41;](~/database-engine/log-shipping/about-log-shipping-sql-server.md)  
  
