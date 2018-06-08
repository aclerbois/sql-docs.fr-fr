---
title: Définir la fréquence d’interrogation pour les serveurs cibles | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssms-agent
ms.reviewer: ''
ms.suite: sql
ms.technology: ssms
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- interval for polling [SQL Server]
- target servers [SQL Server], polling interval
- polling interval [SQL Server]
ms.assetid: 4ffbbefa-77fb-442e-a77c-cb8c6cab9f3c
caps.latest.revision: 5
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-mi-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: fa6df410f4420dffd53160ff3c0c9b91a00b2c9f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="set-the-polling-interval-for-target-servers"></a>Set the Polling Interval for Target Servers
[!INCLUDE[appliesto-ss-asdbmi-xxxx-xxx-md](../../includes/appliesto-ss-asdbmi-xxxx-xxx-md.md)]

> [!IMPORTANT]  
> Dans [Azure SQL Database Managed Instance](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance), la plupart des fonctionnalités SQL Server Agent sont prises en charge. Pour plus d’informations, consultez [Différences T-SQL entre Azure SQL Database Managed Instance et SQL Server](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Cette rubrique explique comment définir la fréquence avec laquelle l'Agent [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] actualise les informations du serveur maître sur les serveurs cibles. Un travail est une série d'actions exécutées par [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent. Un travail multiserveur est un travail exécuté par un serveur maître sur un ou plusieurs serveurs cibles.  
  
-   **Avant de commencer :**  [Sécurité](#Security)  
  
-   **Pour définir l’intervalle d’interrogation pour les serveurs cibles, avec :** [SQL Server Management Studio](#SSMS), [Transact-SQL](#TSQL)  
  
## <a name="BeforeYouBegin"></a>Avant de commencer  
Chaque serveur cible peut exécuter une instance du même travail au même moment. Chaque serveur cible interroge régulièrement le serveur maître, télécharge une copie des nouveaux travaux affectés au serveur cible, puis se déconnecte. Le serveur cible exécute localement le travail, puis se reconnecte au serveur maître pour charger l'état de la sortie du travail.  
  
> [!NOTE]  
> Si le serveur maître est accessible lorsque le serveur cible tente de charger l'état du travail, ce dernier est mis en attente dans le spouleur jusqu'à ce que le serveur maître soit accessible.  
  
### <a name="Security"></a>Sécurité  
Pour plus d'informations, consultez [Implement SQL Server Agent Security](../../ssms/agent/implement-sql-server-agent-security.md) et [Choose the Right SQL Server Agent Service Account for Multiserver Environments](../../ssms/agent/choose-the-right-sql-server-agent-service-account-for-multiserver-environments.md).  
  
## <a name="SSMS"></a>Utilisation de SQL Server Management Studio  
**Pour définir l'intervalle d'interrogation pour les serveurs cibles**  
  
1.  Dans **l’Explorateur d'objets** , connectez-vous à une instance du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion_md.md)]et développez-la.  
  
2.  Cliquez avec le bouton droit sur **Agent SQL Server**, pointez sur **Administration multiserveur**, puis cliquez sur **Gérer les serveurs cibles**.  
  
3.  Sous l'onglet **État du serveur cible** , cliquez sur **Publier les instructions**.  
  
4.  Dans la liste **Type d'instruction** , sélectionnez **Définir la fréquence d'interrogation**.  
  
5.  Dans la zone **Fréquence d'interrogation** , entrez le nombre de secondes, entre 10 et 28800, qui doivent s'écouler avant que le serveur cible n'interroge le serveur maître.  
  
6.  Sous **Destinataires**, activez l'une des options suivantes :  
  
    1.  Cliquez sur **Tous les serveurs cibles** si tous les serveurs cibles partagent la même fréquence d'interrogation.  
  
    2.  Cliquez sur **Serveurs cibles sélectionnés** si les serveurs cibles ne partagent pas tous la même fréquence d'interrogation, puis sélectionnez chacun des serveurs cibles qui utilisera cette fréquence d'interrogation.  
  
## <a name="TSQL"></a>Utilisation de Transact-SQL  
**Pour définir l'intervalle d'interrogation pour les serveurs cibles**  
  
1.  Dans l'Explorateur d'objets, connectez-vous à une instance du moteur de base de données et développez-la.  
  
2.  Dans la barre d'outils, cliquez sur **Nouvelle requête**.  
  
3.  Dans la fenêtre de requête, utilisez la procédure stockée système [sp_post_msx_operation (Transact-SQL)](http://msdn.microsoft.com/en-us/085deef8-2709-4da9-bb97-9ab32effdacf) pour définir la fréquence d’interrogation des serveurs cibles.  
  
## <a name="see-also"></a> Voir aussi  
[sysdownloadlist](http://msdn.microsoft.com/en-us/71087a4c-e829-488e-aa7d-a9476e2b4779)  
  
