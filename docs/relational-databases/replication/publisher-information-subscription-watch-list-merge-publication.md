---
title: Informations sur le serveur de publication, Liste de suivi des abonnements (Publication de fusion) | Microsoft Docs
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
f1_keywords:
- sql13.rep.monitor.publisherinfo.subscriptionssummary.merge.f1
ms.assetid: 4ec956bf-5cef-4377-a1d1-8c7f0107a6cb
caps.latest.revision: 32
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: f24a549196cfe7d795a4ae0b0f8b278cc3eb008a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="publisher-information-subscription-watch-list-merge-publication"></a>Informations sur le serveur de publication, Liste de suivi des abonnements (Publication de fusion)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  L'onglet **Liste de suivi des abonnements** est disponible pour les serveurs de distribution qui exécutent [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] et les versions ultérieures. Il permet d'afficher des informations sur les abonnements depuis toutes les publications disponibles sur le serveur de publication sélectionné. Vous pouvez filtrer la liste des abonnements pour identifier les erreurs, les avertissements et les abonnements qui ne fonctionnent pas correctement. Cet onglet fournit à l'administrateur un emplacement central pour contrôler toute l'activité de réplication sur un serveur de publication : le Moniteur de réplication affiche tous les abonnements nécessitant une attention, en fonction du type de réplication sélectionné et de l'option choisie dans la zone de liste déroulante **Afficher** . Du fait que les éléments affichés dans cet onglet reposent sur l'état et les performances actuelles, les abonnements sont affichés sur cette page uniquement s'ils correspondent à l'option actuelle de la zone de liste **Afficher** .  
  
## <a name="options"></a>Options  
 Pour plus d'informations et en savoir plus sur les tâches associées à un abonnement, cliquez avec le bouton droit de la souris sur la ligne de l'abonnement, puis cliquez sur une option dans le menu contextuel. Pour modifier la façon dont la grille affiche les données, cliquez avec le bouton droit sur la grille, puis cliquez sur l'une des options suivantes :  
  
-   **Trier**: cette option vous permet d'effectuer un tri sur ou plusieurs colonnes dans la boîte de dialogue **Trier les colonnes** .  
  
-   **Choisir les colonnes à afficher**: cette option vous permet de sélectionner les colonnes à afficher et l'ordre d'affichage dans la boîte de dialogue **Choisir les colonnes** .  
  
-   **Filtre**: cette option vous permet de filtrer les lignes dans la grille selon les valeurs de colonne dans la boîte de dialogue **Paramètres du filtre** .  
  
-   **Effacer le filtre**: cette option vous permet d'effacer tous les paramètres du filtre pour la grille.  
  
 Les paramètres du filtre sont spécifiques à chaque grille. La sélection et le tri des colonnes sont appliqués à toutes les grilles du même type, par exemple la grille de publications pour chaque serveur de publication.  
  
 **Afficher les abonnements de fusion**  
 Sélectionnez le type des abonnements (transactionnel, instantané ou fusion) à afficher pour le serveur de publication sélectionné.  
  
 **Afficher**  
 Sélectionnez les états d'abonnement à afficher pour le type d'abonnement sélectionné. Par exemple, vous pouvez afficher uniquement les abonnements associés à une erreur.  
  
 **État**  
 État de chaque abonnement déterminé par l'état de l'Agent de fusion.  
  
 Par défaut, la grille qui contient des informations d'abonnement est triée en fonction de la colonne **État** (puis en fonction de la colonne **Performances** pour les abonnements ayant le même état). La liste suivante contient toutes les valeurs d'état possibles et l'ordre de tri des valeurs (par exemple, les erreurs sont toujours affichées dans la partie supérieure de la grille).  
  
-   Error  
  
-   Critique pour les performances  
  
-   Fusion longue  
  
-   Expire bientôt/Expiré  
  
-   Abonnement non initialisé  
  
-   Nouvelle tentative de la commande qui a échoué  
  
-   Synchronisation  
  
-   Sans synchronisation  
  
 L'ordre de tri détermine également la valeur affichée lorsqu'un abonnement a plusieurs états. Par exemple, si un abonnement comporte une erreur et expire bientôt, la colonne **État** affiche **Erreur**.  
  
 Les valeurs d'état **Critique pour les performances**, **Fusion longue**, **Expire bientôt/Expiré**et **Abonnement non initialisé** sont des avertissements. Lorsqu'un avertissement est affiché, la colonne **État** s'affiche également si un agent est en cours de synchronisation. Par exemple, l'état peut être **Synchronisation, Critique pour les performances**.  
  
 Les valeurs d'état **Expire bientôt/Expiré** et **Fusion longue** ne s'affichent que si des seuils sont définis. La valeur d'état **Critique pour les performances** ne peut s'afficher qu'après cinq synchronisations d'abonnements avec le même type de connexion (accès à distance ou LAN). Pour plus d’informations sur les mesures de performances et sur la définition des seuils, consultez [Analyser les performances avec le Moniteur de réplication](../../relational-databases/replication/monitor/monitor-performance-with-replication-monitor.md) et [Définir des seuils et des avertissements dans le Moniteur de réplication](../../relational-databases/replication/monitor/set-thresholds-and-warnings-in-replication-monitor.md).  
  
 **Abonnement**  
 Nom de chaque abonnement, au format :*NomAbonné: NomBaseDonnéesAbonnements*.  
  
 **Nom convivial**  
 Description de chaque abonnement. La description est entrée dans la boîte de dialogue **Propriétés de l'abonnement** ou définie avec le paramètre **@description** de [sp_addmergesubscription](../../relational-databases/system-stored-procedures/sp-addmergesubscription-transact-sql.md) ou de [sp_addmergepullsubscription](../../relational-databases/system-stored-procedures/sp-addmergepullsubscription-transact-sql.md). Les utilisateurs utilisent souvent la description sous la forme d'un « nom convivial » ou d'un alias pour l'abonnement.  
  
 **Publication**  
 Nom de la publication avec laquelle un abonnement se synchronise, au format : *NomBaseDonnéesPublication: NomPublication*.  
  
 **Performances**  
 Valeur de performance de chaque abonnement en fonction de la dernière mesure de vitesse de transmission relevée par le Moniteur de réplication. L'évaluation repose sur une comparaison des performances individuelles d'abonnements avec les performances historiques moyennes des abonnements à la publication au type de connexion identique (accès à distance ou LAN). Le moniteur de réplication affiche une valeur après cinq synchronisations avec au moins 50 modifications chacune via le même type de connexion. S'il y a eu moins de cinq synchronisations avec au moins 50 modifications ou que la synchronisation la plus récente possède moins de 50 modifications, cette colonne est vide.  
  
> [!NOTE]  
>  Les performances sont basées sur le type de connexion affiché dans la colonne **Connexion** . Par conséquent, il est possible qu'un abonnement ayant une vitesse de transmission inférieure affiche une meilleure valeur de performance qu'un autre abonnement si le premier abonnement est synchronisé sur une connexion plus lente.  
  
 Les valeurs possibles sont les suivantes :  
  
-   Excellent  
  
-   Bonne  
  
-   Correcte  
  
-   Médiocre  
  
 Pour plus d’informations sur la définition des indices et des seuils de performances, consultez [Analyser les performances avec le Moniteur de réplication](../../relational-databases/replication/monitor/monitor-performance-with-replication-monitor.md).  
  
 **Vitesse de transmission**  
 Nombre de lignes que traite l'Agent de fusion chaque seconde.  
  
 **Dernière synchronisation**  
 Heure de la dernière exécution de l'Agent de fusion. Les modifications peuvent avoir été traitées ou non au cours de la synchronisation. Si la synchronisation est en cours, le pourcentage d'avancement s'affiche.  
  
 **Duration**  
 Délai d'exécution de l'Agent de fusion au cours de la dernière synchronisation. La durée correspond au délai écoulé si l'Agent de fusion est en cours de synchronisation ou au délai total si l'Agent s'est déjà synchronisé.  
  
 **Connexion**  
 Type de connexion entre l'Abonné et le serveur de publication. Les valeurs possibles sont **LAN**, **Connexion à distance**et **Internet**. La valeur **Internet** s'affiche si l'abonnement utilise la synchronisation Web.  
  
## <a name="see-also"></a> Voir aussi  
 [Démarrer le Moniteur de réplication](../../relational-databases/replication/monitor/start-the-replication-monitor.md)   
 [Afficher des informations et exécuter des tâches pour un serveur de publication &#40;moniteur de réplication&#41;](../../relational-databases/replication/monitor/view-information-and-perform-tasks-for-a-publisher-replication-monitor.md)   
 [Surveillance de la réplication](../../relational-databases/replication/monitor/monitoring-replication-overview.md)   
 [Synchronisation web pour la réplication de fusion](../../relational-databases/replication/web-synchronization-for-merge-replication.md)  
  
  
