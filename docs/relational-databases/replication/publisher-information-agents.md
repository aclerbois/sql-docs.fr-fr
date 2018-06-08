---
title: Informations sur le serveur de publication, Agents | Microsoft Docs
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
- sql13.rep.monitor.publisherinfo.commonjobs.f1
ms.assetid: 2346c00d-c269-45a1-af14-68e7fd7ebd7e
caps.latest.revision: 26
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 1e540001db7e0ce14a951f3d150537886a617685
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="publisher-information-agents"></a>Informations sur le serveur de publication, Agents
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  L'onglet **Agents** affiche des informations sur les agents et les travaux de maintenance associés au serveur de publication :  
  
-   Agent d'instantané, affiché pour toutes les publications  
  
-   Agent de lecture du journal, affiché pour toutes les publications transactionnelles  
  
-   Agent de lecture de la file d'attente, affiché pour les publications transactionnelles qui ont des abonnements mis à jour en file d'attente  
  
-   Travaux de maintenance, affichés pour toutes les publications :  
  
    -   Réinitialiser les abonnements présentant des erreurs lors la validation de données  
  
    -   Nettoyage de l'historique de l'Agent : distribution  
  
    -   Actualisateur d'analyse de réplication pour la distribution  
  
    -   Contrôle des agents de réplication  
  
    -   Nettoyage de distribution : distribution  
  
    -   Nettoyage de l'abonnement expiré  
  
 Pour plus d’informations sur ces travaux, consultez [Administration de l’Agent de réplication](../../relational-databases/replication/agents/replication-agent-administration.md).  
  
## <a name="options"></a>Options  
 Pour afficher des informations sur un agent ou un travail, sélectionnez l'option de votre choix dans le menu déroulant **Types d'agents et de travaux** . Pour des informations plus détaillées et obtenir des tâches en relation avec un Agent ou un travail, cliquez avec le bouton droit sur la ligne correspondant à cet Agent ou à ce travail, puis cliquez sur une option dans le menu contextuel. Pour modifier la façon dont la grille affiche les données, cliquez avec le bouton droit sur la grille, puis cliquez sur l'une des options suivantes :  
  
-   **Trier**: cette option vous permet d'effectuer un tri sur ou plusieurs colonnes dans la boîte de dialogue **Trier les colonnes** .  
  
-   **Choisir les colonnes à afficher**: cette option vous permet de sélectionner les colonnes à afficher et l'ordre d'affichage dans la boîte de dialogue **Choisir les colonnes** .  
  
-   **Filtre**: cette option vous permet de filtrer les lignes dans la grille selon les valeurs de colonne dans la boîte de dialogue **Paramètres du filtre** .  
  
-   **Effacer le filtre**: cette option vous permet d'effacer tous les paramètres du filtre pour la grille.  
  
 Les paramètres du filtre sont spécifiques à chaque grille. La sélection et le tri des colonnes sont appliqués à toutes les grilles du même type, par exemple la grille de publications pour chaque serveur de publication.  
  
 Les sections suivantes décrivent les données qui sont affichées sous cet onglet pour chaque agent ou travail.  
  
### <a name="snapshot-agent"></a>Agent d'instantané  
 **État**  
 État de l'Agent. La liste ci-dessous indique les valeurs d'état possibles :  
  
-   Error  
  
-   Réessayer  
  
-   Exécution en cours  
  
-   Terminé  
  
 **Publication**  
 Nom de la publication à laquelle l'agent est associé.  
  
 **Dernière heure de début**  
 Dernière heure à laquelle l'Agent a démarré.  
  
 **Duration**  
 Durée pendant laquelle l'agent s'est exécuté. Cette durée représente le temps actuel si l'agent est en cours d'exécution ou le temps total d'exécution s'il a été exécuté et s'est terminé.  
  
 **Dernière action**  
 La dernière action effectuée au cours de la dernière exécution de l'Agent.  
  
 **Vitesse de transmission**  
 Vitesse, en commandes par seconde, à laquelle les commandes d'initialisation sont validées dans la base de données de distribution lors de la dernière exécution de l'Agent.  
  
 **#Trans**  
 Nombre de transactions validées dans la base de données de distribution lors de la dernière exécution de l'Agent.  
  
 **#Cmds**  
 Nombre de commandes validées dans la base de données de distribution lors de la dernière exécution de l'Agent. Une commande est équivalente à une modification des données, par exemple une mise à jour.  
  
### <a name="log-reader-agent"></a>l'Agent de lecture du journal ;  
 **État**  
 État de l'Agent. La liste ci-dessous indique les valeurs d'état possibles :  
  
-   Error  
  
-   Réessayer  
  
-   Exécution en cours  
  
-   Non exécuté  
  
 **Base de données de publication**  
 Nom de la publication à laquelle l'agent est associé.  
  
 **Dernière heure de début**  
 Dernière heure à laquelle l'Agent a démarré.  
  
 **Duration**  
 Durée pendant laquelle l'agent s'est exécuté. Cette durée représente le temps actuel si l'agent est en cours d'exécution ou le temps total d'exécution s'il a été exécuté et s'est terminé.  
  
 **Dernière action**  
 La dernière action effectuée au cours de la dernière exécution de l'Agent.  
  
 **Vitesse de transmission**  
 Vitesse, en commandes par seconde, à laquelle les modifications sont validées dans la base de données de distribution.  
  
 **Latence**  
 Durée, en secondes, écoulée entre la modification la plus récente validée dans la base de données de publication et la commande correspondante validée dans la base de données de distribution.  
  
 **#Trans**  
 Nombre de transactions validées dans la base de données de distribution lors de la dernière exécution de l'Agent.  
  
 **#Cmds**  
 Nombre de commandes validées dans la base de données de distribution lors de la dernière exécution de l'Agent. Une commande est équivalente à une modification des données, par exemple une mise à jour.  
  
 **Nb moy. de #Cmds**  
 Nombre moyen de commandes par transaction lors de la dernière exécution de l'Agent.  
  
### <a name="queue-reader-agent"></a>Agent de lecture de la file d'attente  
 **État**  
 État de l'Agent. La liste ci-dessous indique les valeurs d'état possibles :  
  
-   Error  
  
-   Réessayer  
  
-   Exécution en cours  
  
-   Non exécuté  
  
 **Base de données de distribution**  
 Nom de la base de données de distribution à laquelle l'Agent est associé.  
  
 **Dernière heure de début**  
 Dernière heure à laquelle l'Agent a démarré.  
  
 **Duration**  
 Durée pendant laquelle l'agent s'est exécuté. La durée correspond au délai écoulé si l'Agent est actif ou au délai total si l'Agent a déjà été exécuté.  
  
 **Dernière action**  
 La dernière action effectuée au cours de la dernière exécution de l'Agent.  
  
 **Vitesse de transmission**  
 Vitesse, en commandes par seconde, à laquelle les modifications sont validées dans la base de données de distribution.  
  
 **Latence**  
 Durée, en secondes, écoulée entre la modification la plus récente validée dans une base de données d'abonnement et la commande correspondante validée dans la base de données de publication.  
  
 **#Trans**  
 Nombre de transactions validées dans la base de données de publication lors de la dernière exécution de l'Agent.  
  
 **#Cmds**  
 Nombre de commandes validées dans la base de données de publication lors de la dernière exécution de l'Agent. Une commande est équivalente à une modification des données, par exemple une mise à jour.  
  
 **Nb moy. de #Cmds**  
 Nombre moyen de commandes par transaction lors de la dernière exécution de l'Agent.  
  
### <a name="maintenance-jobs"></a>Travaux de maintenance  
 **État**  
 État de chaque travail. La liste ci-dessous indique les valeurs d'état possibles :  
  
-   Error  
  
-   Réessayer  
  
-   Exécution en cours  
  
-   Non exécuté  
  
 **Travail**  
 Nom du travail.  
  
 **Dernière heure de début**  
 Dernière heure à laquelle le travail a démarré.  
  
 **Duration**  
 Durée d'exécution du travail. Cette durée correspond au temps écoulé si le travail est en cours d'exécution et à la durée totale si le travail a été accompli précédemment.  
  
 **Dernière action**  
 Dernière action effectuée lors de la dernière exécution du travail.  
  
## <a name="see-also"></a> Voir aussi  
 [Démarrer le Moniteur de réplication](../../relational-databases/replication/monitor/start-the-replication-monitor.md)   
 [Afficher des informations et exécuter des tâches pour un serveur de publication &#40;moniteur de réplication&#41;](../../relational-databases/replication/monitor/view-information-and-perform-tasks-for-a-publisher-replication-monitor.md)   
 [Afficher des informations et effectuer des tâches pour les agents associés à une publication &#40;moniteur de réplication&#41;](../../relational-databases/replication/monitor/view-information-and-perform-tasks-for-publication-agents.md)   
 [Surveillance de la réplication](../../relational-databases/replication/monitor/monitoring-replication-overview.md)  
  
  
