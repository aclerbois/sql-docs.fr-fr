---
title: Informations de publication, jetons de suivi (SQL Server 2005 et versions ultérieures) | Microsoft Docs
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
- sql13.rep.monitor.publicationinfo.tracertokens.f1
ms.assetid: a115ba95-17ae-45df-91bd-5a1a35f3745f
caps.latest.revision: 24
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 6c51d1d63101f360eb3c1e30a83828a38593cf14
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="publication-information-tracer-tokens-sql-server-2005-and-later"></a>Informations de publication, jetons de suivi (SQL Server 2005 et versions ultérieures)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  L'onglet **Jetons de suivi** vous permet de contrôler la validité des connexions mais aussi de paramétrer la mesure du temps de latence d'un système utilisant la réplication transactionnelle. Un jeton (une petite quantité de données) est écrit dans le journal des transactions de la base de données de publication et est marqué comme s'il s'agissait d'une transaction standard répliquée, puis est envoyé dans le système, permettant ainsi le calcul :  
  
-   du temps écoulé entre la validation d'une transaction par le serveur de publication et l'insertion de la commande qui en découle dans la base de données de distribution sur le serveur de distribution ;  
  
-   du temps écoulé entre l'insertion d'une commande dans la base de données de distribution et la validation de la transaction qui en découle au niveau d'un Abonné.  
  
 De par ces calculs, vous pouvez ainsi répondre à diverses questions, notamment :  
  
-   Quels Abonnés mettent le plus longtemps à recevoir une modification du serveur de publication ?  
  
-   De l'ensemble des Abonnés prévus pour recevoir le jeton de suivi, lesquels, le cas échéant, ne l'ont pas reçu ?  
  
## <a name="options"></a>Options  
 Pour modifier la façon dont la grille affiche les données, cliquez avec le bouton droit sur la grille, puis cliquez sur l'une des options suivantes :  
  
-   **Trier**: cette option vous permet d'effectuer un tri sur ou plusieurs colonnes dans la boîte de dialogue **Trier les colonnes** .  
  
-   **Choisir les colonnes à afficher**: cette option vous permet de sélectionner les colonnes à afficher et l'ordre d'affichage dans la boîte de dialogue **Choisir les colonnes** .  
  
-   **Filtre**: cette option vous permet de filtrer les lignes dans la grille selon les valeurs de colonne dans la boîte de dialogue **Paramètres du filtre** .  
  
-   **Effacer le filtre**: cette option vous permet d'effacer tous les paramètres du filtre pour la grille.  
  
 Les paramètres du filtre sont spécifiques à chaque grille. La sélection et le tri des colonnes sont appliqués à toutes les grilles du même type, par exemple la grille de publications pour chaque serveur de publication.  
  
 **Insérer un suivi**  
 Permet d'insérer en cliquant sur un jeton de suivi dans le journal des transactions sur le serveur de publication.  
  
 **Heure de l'insertion**  
 Permet de spécifier une heure à laquelle un jeton de suivi a été inséré afin d'afficher les informations relatives au temps de latence par rapport à cette heure. Par défaut, les informations du temps le plus récent sont celles affichées.  
  
> [!NOTE]  
>  Les informations de jeton de suivi sont conservées pour la même durée que toute autre donnée d'historique, elles-mêmes étant régies par la période de rétention des historiques définie dans la base de données de distribution. Pour plus d’informations sur la modification des propriétés de base de données de distribution, consultez [Afficher et modifier les propriétés d’un serveur de distribution ou d’un serveur de publication](../../relational-databases/replication/view-and-modify-distributor-and-publisher-properties.md).  
  
 **Abonnement**  
 Nom de chaque abonnement à la publication.  
  
 **Du serveur de publication vers le serveur de distribution**  
 Temps écoulé entre la validation d'une transaction par le serveur de publication et l'insertion de la commande qui en découle dans la base de données de distribution sur le serveur de distribution. La valeur **En attente** indique que le jeton n'est pas encore arrivé au serveur de distribution. Si cet état ne semble pas évoluer, vérifiez que l'Agent de lecture du journal est bien en cours d'exécution.  
  
 **Du serveur de distribution vers l'Abonné**  
 Temps écoulé entre l'insertion d'une commande dans la base de données de distribution et la validation de la transaction qui en découle au niveau d'un Abonné. La valeur **En attente** indique que le jeton n'est pas encore arrivé à l'Abonné. Si cet état persiste, vérifiez que l'Agent de distribution est bien en cours d'exécution.  
  
 **Latence totale**  
 Temps écoulé entre la validation d'une transaction par le serveur de publication et la validation de la transaction qui en découle au niveau de l'Abonné. Cette option représente la latence du système de réplication pour un abonné donné au moment présent, du début de la procédure à sa fin. La valeur **En attente** indique que le jeton n'est pas encore arrivé à l'Abonné.  
  
## <a name="see-also"></a> Voir aussi  
 [Démarrer et arrêter un Agent de réplication &#40;SQL Server Management Studio&#41;](../../relational-databases/replication/agents/start-and-stop-a-replication-agent-sql-server-management-studio.md)   
 [Démarrer le Moniteur de réplication](../../relational-databases/replication/monitor/start-the-replication-monitor.md)   
 [Mesurer la latence et valider les connexions pour la réplication transactionnelle](../../relational-databases/replication/monitor/measure-latency-and-validate-connections-for-transactional-replication.md)   
 [Analyser les performances avec le Moniteur de réplication](../../relational-databases/replication/monitor/monitor-performance-with-replication-monitor.md)   
 [Surveillance de la réplication](../../relational-databases/replication/monitor/monitoring-replication-overview.md)   
 [Présentation des Agents de réplication](../../relational-databases/replication/agents/replication-agents-overview.md)  
  
  
