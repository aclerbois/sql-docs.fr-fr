---
title: Groupe de disponibilité - Assistant Ajout d’une base de données à un groupe | Microsoft Docs
ms.custom: ''
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: high-availability
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.swb.adddatabasewizard.f1
helpviewer_keywords:
- Availability Groups [SQL Server], wizards
- Availability Groups [SQL Server], databases
ms.assetid: 81e5e36d-735d-4731-8017-2654673abb88
caps.latest.revision: 27
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: e836fb404f71d1e2a37a7ab91353b4734aa50484
ms.sourcegitcommit: 8aa151e3280eb6372bf95fab63ecbab9dd3f2e5e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34769285"
---
# <a name="availability-group---add-database-to-group-wizard"></a>Groupe de disponibilité - Assistant Ajout d’une base de données à un groupe
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Utilisez l’Assistant Ajouter une base de données au groupe de disponibilité pour ajouter une ou plusieurs bases de données à un groupe de disponibilité Always On existant.  
  
> [!NOTE]  
>  Pour plus d’informations sur l’utilisation de [!INCLUDE[tsql](../../../includes/tsql-md.md)] ou de PowerShell pour ajouter une base de données, consultez [Ajouter une base de données à un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/availability-group-add-a-database.md).  
  
 **Dans cette rubrique :**  
  
-   **Avant de commencer :**  
  
     [Conditions préalables requises et restrictions](#Prerequisites)  
  
     [Sécurité](#Security)  
  
-   **Pour ajouter une base de données, utilisez :**  [Assistant Ajouter une base de données au groupe de disponibilité (SQL Server Management Studio)](#SSMSProcedure)  
  
##  <a name="BeforeYouBegin"></a> Avant de commencer  
 Si vous n’avez jamais ajouté de base de données à un groupe de disponibilité, consultez la section « Bases de données de disponibilité » dans [Conditions préalables requises, restrictions et recommandations pour les groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md).  
  
###  <a name="Prerequisites"></a> Conditions préalables requises, restrictions et recommandations  
  
-   Vous devez être connecté à l'instance de serveur qui héberge le réplica principal actuel.  
  
-   **Conditions préalables requises pour utiliser la synchronisation de données initiale complète**  
  
    -   Tous les chemins d'accès des fichiers de base de données doivent être identiques sur chaque instance de serveur qui héberge un réplica pour le groupe de disponibilité.  
  
    -   Aucun nom de base de données primaire ne peut exister sur une instance de serveur qui héberge un réplica secondaire. Cela signifie qu'aucune des nouvelles bases de données secondaires ne peut exister pour le moment.  
  
    -   Vous devez spécifier un partage réseau pour que l'assistant crée des sauvegardes et puisse y accéder. Pour le réplica principal, le compte utilisé pour démarrer le [!INCLUDE[ssDE](../../../includes/ssde-md.md)] doit disposer d'autorisations de système de fichiers en lecture et en écriture sur un partage réseau. Pour les réplicas secondaires, le compte doit disposer d'une autorisation en lecture sur le partage réseau.  
  
     Si vous ne pouvez pas utiliser l'Assistant pour effectuer la synchronisation des données initiale complète, vous devez préparer vos bases de données secondaires manuellement. Vous pouvez le faire avant ou après l'exécution de l'Assistant. Pour plus d’informations, consultez [Préparer manuellement une base de données secondaire pour un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md).  
  
###  <a name="Security"></a> Sécurité  
  
####  <a name="Permissions"></a> Permissions  
 Requiert l'autorisation ALTER AVAILABILITY GROUP sur le groupe de disponibilité, l'autorisation CONTROL AVAILABILITY GROUP, l'autorisation ALTER ANY AVAILABILITY GROUP ou l'autorisation CONTROL SERVER.  
  
##  <a name="SSMSProcedure"></a> Utilisation de l'Assistant Ajouter une base de données au groupe de disponibilité (SQL Server Management Studio)  
 **Pour utiliser l'Assistant Ajouter une base de données au groupe de disponibilité**  
  
1.  Dans l'Explorateur d'objets, connectez-vous à l'instance de serveur qui héberge le réplica principal du groupe de disponibilité et développez l'arborescence du serveur.  
  
2.  Développez le nœud **Haute disponibilité AlwaysOn** et le nœud **Groupes de disponibilité** .  
  
3.  Cliquez avec le bouton droit sur le groupe de disponibilité auquel vous ajoutez une base de données, puis sélectionnez la commande **Ajouter une base de données** . Cette commande lance l'Assistant Ajouter une base de données au groupe de disponibilité.  
  
4.  Sur la page **Sélectionner les bases de données** , sélectionnez une ou plusieurs bases de données. Pour plus d’informations, consultez [Sélectionner les bases de données &#40;Assistant Nouveau groupe de disponibilité et Assistant Ajouter une base de données&#41;](../../../database-engine/availability-groups/windows/select-databases-page-new-availability-group-wizard-and-add-database-wizard.md).  
  
     Si la base de données contient une clé principale de base de données, entrez le mot de passe de la clé principale de base de données dans la colonne **Mot de passe** .  
  
5.  Sur la page **Sélectionner la synchronisation de données initiale** , choisissez comment vous souhaitez que vos nouvelles bases de données secondaires soient créées et jointes au groupe de disponibilité. Choisissez l'une des options suivantes :  
  
    -   **Complet**  
  
         Sélectionnez cette option si votre environnement répond aux conditions requises pour démarrer automatiquement la synchronisation initiale des données (pour plus d’informations, consultez [Conditions préalables requises, restrictions et recommandations](#Prerequisites), plus haut dans cette rubrique).  
  
         Si vous sélectionnez **Complet**, après avoir créé le groupe de disponibilité, l'assistant tente de sauvegarder chaque base de données primaire et son journal des transactions sur un partage réseau et restaure les sauvegardes sur chaque instance de serveur qui héberge un réplica secondaire. L'assistant joint ensuite chaque base de données secondaire au groupe de disponibilité.  
  
         Dans le champ **Spécifier un emplacement réseau partagé accessible par tous les réplicas** , spécifiez un partage de sauvegarde dans lequel l’intégralité de l’instance de serveur qui héberge les réplicas dispose d’un accès en lecture-écriture. Les sauvegardes de journaux feront partie de votre chaîne de sauvegarde du journal. Stockez les fichiers de sauvegarde des journaux de manière appropriée.  
  
        > [!IMPORTANT]  
        >  Pour plus d’informations sur les autorisations de système de fichiers requises, consultez [Configuration préalable requise](#Prerequisites)plus haut dans cette rubrique.  
  
    -   **Joindre uniquement**  
  
         Si vous avez préparé manuellement les bases de données secondaires sur les instances de serveur qui hébergeront les réplicas secondaires, vous pouvez sélectionner cette option. L'assistant joindra les bases de données secondaires existantes au groupe de disponibilité.  
  
    -   **Ignorer la synchronisation de données initiale**  
  
         Sélectionnez cette option si vous souhaitez utiliser votre propre base de données et sauvegardes de journaux de vos bases de données primaires. Pour plus d’informations, consultez [Démarrer un mouvement de données sur une base de données secondaire Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/start-data-movement-on-an-always-on-secondary-database-sql-server.md).  
  
     Pour plus d’informations, consultez [Sélectionner la synchronisation de données initiale &#40;Assistants de groupe de disponibilité Always On&#41;](../../../database-engine/availability-groups/windows/select-initial-data-synchronization-page-always-on-availability-group-wizards.md).  
  
6.  Sur la page **Se connecter à des réplicas secondaires existants** , si les instances de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] qui hébergent les réplicas de disponibilité pour ce groupe de disponibilité sont toutes exécutées en tant que service dans le même compte d'utilisateur, cliquez sur **Se connecter à tous**. Si l'une des instances de serveur s'exécute en tant que service sous des comptes différents, cliquez sur le bouton **Se connecter** individuel figurant à droite du nom de chaque instance de serveur.  
  
     Pour plus d’informations, consultez [Se connecter à la page à des réplicas secondaires existants &#40;Assistant Ajout de réplica : Assistant Ajout de bases de données&#41;](../../../database-engine/availability-groups/windows/connect-to-existing-secondary-replicas-page.md).  
  
7.  La page **Validation** vérifie si les valeurs que vous avez spécifiées dans cet Assistant répondent aux exigences de l'Assistant Nouveau groupe de disponibilité. Pour effectuer un changement, vous pouvez cliquer sur **Précédent** pour revenir à une page antérieure de l'assistant pour modifier une ou plusieurs valeurs. Cliquez sur **Suivant** pour revenir à la page **Validation**, puis cliquez sur **Réexécuter la validation**.  
  
     Pour plus d’informations, consultez [Page Validation &#40;Assistants de groupe de disponibilité Always On&#41;](../../../database-engine/availability-groups/windows/validation-page-always-on-availability-group-wizards.md).  
  
8.  Sur la page **Résumé** , examinez vos choix pour le nouveau groupe de disponibilité. Pour apporter une modification, cliquez sur **Précédent** pour revenir à la page appropriée. Après avoir apporté la modification, cliquez sur **Suivant** pour revenir à la page **Résumé** .  
  
     Pour plus d’informations, consultez [Page Résumé &#40;Assistants de groupe de disponibilité Always On&#41;](../../../database-engine/availability-groups/windows/summary-page-always-on-availability-group-wizards.md).  
  
     Si vous êtes satisfait de vos sélections, cliquez éventuellement sur Script pour créer un script des étapes que l'assistant devra exécuter. Ensuite, pour créer et configurer le nouveau groupe de disponibilité, cliquez sur **Terminer**.  
  
9. La page **État d’avancement** affiche l’état d’avancement des étapes de création du groupe de disponibilité (configuration de points de terminaison, création du groupe de disponibilité et jointure du réplica secondaire au groupe).  
  
     Pour plus d’informations, consultez [Page Progression &#40;Assistants de groupe de disponibilité Always On&#41;](../../../database-engine/availability-groups/windows/progress-page-always-on-availability-group-wizards.md).  
  
10. Lorsque ces étapes sont terminées, la page **Résultats** affiche le résultat de chaque étape. Si toutes ces étapes aboutissent, le nouveau groupe de disponibilité est entièrement configuré. Si l'une des étapes se solde par une erreur, vous devrez peut-être effectuer la configuration manuellement. Pour plus d'informations sur la cause d'une erreur donnée, cliquez sur le lien « Erreur » associé dans la colonne **Résultat** .  
  
     À la fin de l'Assistant, cliquez sur **Fermer** pour le quitter.  
  
     Pour plus d’informations, consultez [Page Résultats &#40;Assistants de groupe de disponibilité Always On&#41;](../../../database-engine/availability-groups/windows/results-page-always-on-availability-group-wizards.md).  
  
11. Si la synchronisation initiale des données n'a pas démarré automatiquement sur toutes vos bases de données secondaires, vous devez configurer toutes les bases de données secondaires non encore jointes. Pour plus d’informations, consultez [Démarrer un mouvement de données sur une base de données secondaire Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/start-data-movement-on-an-always-on-secondary-database-sql-server.md).  
  
##  <a name="RelatedTasks"></a> Tâches associées  
  
-   [Préparer manuellement une base de données secondaire pour un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md)  
  
-   [Joindre une base de données secondaire à un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-database-to-an-availability-group-sql-server.md)  
  
## <a name="see-also"></a> Voir aussi  
 [Vue d’ensemble des groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Conditions préalables requises, restrictions et recommandations pour les groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)   
 [Ajouter une base de données à un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/availability-group-add-a-database.md)   
 [Démarrer un déplacement de données sur une base de données secondaire Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/start-data-movement-on-an-always-on-secondary-database-sql-server.md)   
 [Ajouter une base de données à un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/availability-group-add-a-database.md)  
  
  
