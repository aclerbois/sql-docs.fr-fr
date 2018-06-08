---
title: Ajouter et supprimer des serveurs de publication à partir du moniteur de réplication | Microsoft Docs
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
- Replication Monitor, adding and removing Publishers
ms.assetid: fa36c4b4-bfa5-494e-92e3-07a02d7332c3
caps.latest.revision: 38
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: b0ecf6ff0e925a7eaa01a5197ce71d641591d66e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="add-and-remove-publishers-from-replication-monitor"></a>Ajouter et supprimer des serveurs de publication à partir du Moniteur de réplication
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Le serveur à partir duquel vous lancez le moniteur de réplication est automatiquement ajouté au moniteur s'il s'agit d'un serveur de publication. D'autres serveurs de publication peuvent être ajoutés via la boîte de dialogue **Ajouter un serveur de publication** . Après l'ajout d'un serveur de publication, celui-ci est affiché dans le volet gauche du moniteur. Le groupe **Mes serveurs de publication** est inclus par défaut, mais vous pouvez créer de nouveaux groupes pour gérer une ou plusieurs topologies de réplication. Pour plus d’informations sur le démarrage du Moniteur de réplication, consultez [Démarrer le Moniteur de réplication](../../../relational-databases/replication/monitor/start-the-replication-monitor.md).  
  
### <a name="to-add-a-sql-server-publisher"></a>Pour ajouter un serveur de publication SQL Server  
  
1.  Cliquez avec le bouton droit sur le nœud **Moniteur de réplication** ou sur un nœud de groupe de serveurs de publication, puis cliquez sur **Ajouter un serveur de publication**.  
  
2.  Dans la boîte de dialogue **Ajouter un serveur de publication** , cliquez sur **Ajouter**, puis cliquez sur **Ajouter un serveur de publication SQL Server**.  
  
3.  Dans la boîte de dialogue **Se connecter au serveur** , entrez le nom du serveur de publication, puis sélectionnez le type d'authentification. Si vous sélectionnez **Authentification SQL Server**, entrez un nom de connexion et un mot de passe. Les informations d'identification que vous spécifiez sont enregistrées par le moniteur de réplication pour se connecter à ce serveur dans le futur. Le compte Windows ou le nom de connexion SQL Server spécifié doit être un membre du rôle serveur fixe **sysadmin** ou un membre du rôle de base de données fixe **replmonitor** dans la base de données de distribution.  
  
4.  Cliquez sur **Se connecter**. Si le serveur de publication utilise un serveur de distribution distant, vous serez invité à vous connecter au serveur de distribution dans la boîte de dialogue **Se connecter au serveur** . Les informations d'identification que vous spécifiez sont enregistrées par le moniteur de réplication pour se connecter à ce serveur dans le futur. Le compte Windows ou le nom de connexion SQL Server spécifié doit être un membre du rôle serveur fixe **sysadmin** ou un membre du rôle de base de données fixe **replmonitor** dans la base de données de distribution.  
  
5.  Le nom du serveur de publication et du serveur de distribution sont affichés dans la grille **Démarrer l'analyse du ou des serveurs de publication suivants** .  
  
6.  Pour spécifier des options d'actualisation et de connexion pour le serveur de publication , sélectionnez le serveur de publication dans la grille et modifiez les options comme vous le souhaitez. Pour plus d'informations sur les options d'actualisation, consultez [Caching, Refresh, and Replication Monitor Performance](../../../relational-databases/replication/monitor/caching-refresh-and-replication-monitor-performance.md).  
  
7.  Sélectionnez le groupe sous lequel le serveur de publication doit être affiché dans le moniteur de réplication. Pour créer un nouveau groupe, cliquez sur **Nouveau groupe**, puis entrez un nom de groupe ; sélectionnez le groupe dans la liste **Afficher ce ou ces serveurs de publication dans le groupe suivant** .  
  
8.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
### <a name="to-add-an-oracle-publisher"></a>Pour ajouter un serveur de publication Oracle  
  
1.  Cliquez avec le bouton droit sur le nœud **Moniteur de réplication** ou sur un nœud de groupe de serveurs de publication, puis cliquez sur **Ajouter un serveur de publication**.  
  
2.  Dans la boîte de dialogue **Ajouter un serveur de publication** , cliquez sur **Ajouter**, puis cliquez sur **Ajouter un serveur de publication Oracle**.  
  
3.  Dans la boîte de dialogue **Se connecter au serveur** , entrez le nom du serveur de distribution [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] associé au serveur de publication Oracle, puis sélectionnez le type d'authentification. Si vous sélectionnez **Authentification SQL Server**, entrez un nom de connexion et un mot de passe. Les informations d'identification que vous spécifiez sont enregistrées par le moniteur de réplication pour se connecter à ce serveur dans le futur. Le compte Windows ou le nom de connexion SQL Server spécifié doit être un membre du rôle serveur fixe **sysadmin** ou un membre du rôle de base de données fixe **replmonitor** dans la base de données de distribution.  
  
4.  Cliquez sur **Se connecter**.  
  
5.  Le nom du serveur de publication et du serveur de distribution sont affichés dans la grille **Démarrer l'analyse du ou des serveurs de publication suivants** .  
  
6.  Pour spécifier des options d'actualisation et de connexion pour le serveur de publication , sélectionnez le serveur de publication dans la grille et modifiez les options comme vous le souhaitez. Pour plus d'informations sur les options d'actualisation, consultez [Caching, Refresh, and Replication Monitor Performance](../../../relational-databases/replication/monitor/caching-refresh-and-replication-monitor-performance.md).  
  
7.  Sélectionnez le groupe sous lequel le serveur de publication doit être affiché dans le moniteur de réplication. Pour créer un nouveau groupe, cliquez sur **Nouveau groupe**, puis entrez un nom de groupe ; sélectionnez le groupe dans la liste **Afficher ce ou ces serveurs de publication dans le groupe suivant** .  
  
8.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
### <a name="to-add-one-or-more-publishers-that-use-the-same-distributor"></a>Pour ajouter un ou plusieurs serveurs de publication utilisant le même serveur de distribution  
  
1.  Cliquez avec le bouton droit sur le nœud **Moniteur de réplication** ou sur un nœud de groupe de serveurs de publication, puis cliquez sur **Ajouter un serveur de publication**.  
  
2.  Dans la boîte de dialogue **Ajouter un serveur de publication** , cliquez sur **Ajouter**, puis cliquez sur **Spécifier un serveur de distribution et ajouter ses serveurs de publication**.  
  
3.  Dans la boîte de dialogue **Se connecter au serveur** , entrez le nom du serveur de distribution, puis sélectionnez le type d'authentification. Si vous sélectionnez **Authentification SQL Server**, entrez un nom de connexion et un mot de passe. Les informations d'identification que vous spécifiez sont enregistrées par le moniteur de réplication pour se connecter à ce serveur dans le futur. Le compte Windows ou le nom de connexion SQL Server spécifié doit être un membre du rôle serveur fixe **sysadmin** ou un membre du rôle de base de données fixe **replmonitor** dans la base de données de distribution.  
  
4.  Cliquez sur **Se connecter**.  
  
5.  Le nom du serveur de distribution et celui de chaque serveur de publication sont affichés dans la grille **Démarrer l'analyse du ou des serveurs de publication suivants** . Si un serveur de publication a déjà été ajouté au moniteur de réplication, il n'apparaît pas dans la grille.  
  
6.  Pour spécifier des options d'actualisation et de connexion pour le serveur de publication , sélectionnez le serveur de publication dans la grille et modifiez les options comme vous le souhaitez. Pour plus d'informations sur les options d'actualisation, consultez [Caching, Refresh, and Replication Monitor Performance](../../../relational-databases/replication/monitor/caching-refresh-and-replication-monitor-performance.md).  
  
7.  Sélectionnez le groupe sous lequel les serveurs de publication doivent être affichés dans le moniteur de réplication. Pour créer un nouveau groupe, cliquez sur **Nouveau groupe**, puis entrez un nom de groupe ; sélectionnez le groupe dans la liste **Afficher ce ou ces serveurs de publication dans le groupe suivant** .  
  
8.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
### <a name="to-modify-settings-for-the-publisher-and-publisher-groups"></a>Pour modifier des paramètres pour le serveur de publication et pour les groupes de serveurs de publication  
  
1.  Cliquez avec le bouton droit sur un serveur de publication dans le volet gauche, puis cliquez sur **Paramètres du serveur de publication**.  
  
2.  Effectuez vos modifications dans la boîte de dialogue **Paramètres du serveur de publication** :  
  
    -   Pour modifier les informations d'identification que le moniteur de réplication utilise pour se connecter à un serveur, cliquez sur **Connexion du serveur de publication** ou sur **Connexion du serveur de distribution**, puis entrez les informations d'identification dans la boîte de dialogue **Se connecter au serveur** .  
  
    -   Pour déplacer un serveur de publication d'un groupe à l'autre, sélectionnez le serveur de publication dans la grille **Démarrer l'analyse du ou des serveurs de publication suivants** , puis sélectionnez le nouveau groupe dans **Afficher ce ou ces serveurs de publication dans le groupe suivant** .  
  
3.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
### <a name="to-remove-a-publisher-from-replication-monitor"></a>Pour supprimer un serveur de publication du moniteur de réplication  
  
1.  Cliquez avec le bouton droit sur un serveur de publication dans le volet gauche.  
  
2.  Cliquez sur **Supprimer**.  
  
### <a name="to-add-a-publisher-group-to-replication-monitor"></a>Pour ajouter un groupe de serveurs de publication au moniteur de réplication  
  
1.  Les groupes de serveurs de publication peuvent être créés seulement lors de l'ajout d'un serveur de publication ou de la modification des paramètres d'un serveur de publication. Pour plus d'informations, consultez les procédures concernant l'ajout d'un serveur de publication.  
  
### <a name="to-remove-a-publisher-group-from-replication-monitor"></a>Pour supprimer un groupe de serveurs de publication du moniteur de réplication  
  
1.  Déplacez tous les serveurs de publication vers un autre groupe ou supprimez-les du moniteur de réplication. Pour plus d'informations, consultez les procédures précédentes de cette rubrique.  
  
2.  Cliquez avec le bouton droit sur le groupe de serveurs de publication, puis cliquez sur **Supprimer**.  
  
## <a name="see-also"></a> Voir aussi  
 [Configurer la distribution](../../../relational-databases/replication/configure-distribution.md)   
 [Surveillance de la réplication](../../../relational-databases/replication/monitor/monitoring-replication-overview.md)  
  
  
