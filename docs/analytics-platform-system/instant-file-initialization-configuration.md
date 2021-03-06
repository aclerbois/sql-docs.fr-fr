---
title: "Configuration de l’initialisation instantanée des fichiers (système de plateforme Analytique)"
author: barbkess
ms.author: barbkess
manager: jhubbard
ms.prod: analytics-platform-system
ms.prod_service: mpp-data-warehouse
ms.service: 
ms.component: 
ms.technology: mpp-data-warehouse
ms.custom: 
ms.date: 01/05/2017
ms.reviewer: na
ms.suite: sql
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 58be8982-4d2e-4aa3-bcdd-874a062d2f9d
caps.latest.revision: "20"
ms.openlocfilehash: b7dda4bb925e08f49409ea1950cfe3649b4db3e0
ms.sourcegitcommit: cc71f1027884462c359effb898390c8d97eaa414
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/21/2017
---
# <a name="instant-file-initialization-configuration"></a>Configuration de l’initialisation instantanée des fichiers
Initialisation instantanée des fichiers est une fonctionnalité de SQL Server qui permet les opérations de fichier de données pour s’exécuter plus rapidement. Cochez la case pour activer l’initialisation instantanée de fichiers améliore les performances de SQL Server PDW. Toutefois, si cela pose un risque de sécurité pour vous business, puis laissez la case désactivée.  
  
> [!IMPORTANT]  
> Lors de l’initialisation instantanée des fichiers est activée, SQL Server ne remplace pas les bits supprimés avec des zéros.  Ce comportement peut créer une faille de sécurité si des utilisateurs non autorisés à accéder à des données supprimées. Toutefois, SQL Server PDW permet d’atténuer ce risque en vous assurant que la base de données SQL Server et les fichiers de sauvegarde sont toujours attachés à une instance de SQL Server ; seul le compte de service SQL Server et de l’administrateur local peuvent accéder aux données supprimées sur SQL Server PDW.  
  
L'initialisation instantanée des fichiers n'est pas disponible quand le chiffrement transparent des données est activé.  
  
## <a name="add-permission-for-the-backup-account"></a>Ajoutez l’autorisation pour le compte de sauvegarde  
Le processus de sauvegarde nécessite une information d’identification réseau (compte d’utilisateur Windows) qui peut accéder à l’emplacement de stockage de sauvegarde. Vous autorisez PDW pour utiliser un compte à l’aide de la [sp_pdw_add_network_credentials](../relational-databases/system-stored-procedures/sp-pdw-add-network-credentials-sql-data-warehouse.md) procédure. Consultez [sauvegarde de base de données](../t-sql/statements/backup-database-parallel-data-warehouse.md) pour tout le processus de sauvegarde. Pour utiliser l’initialisation instantanée des fichiers, le compte de sauvegarde doit disposer le `Perform volume maintenance tasks` autorisation.  
  
1.  Sur le serveur de sauvegarde, ouvrez le **stratégie de sécurité locale** application (`secpol.msc`).  
  
2.  Dans le volet gauche, développez **Stratégies locales**, puis cliquez sur **Attribution des droits utilisateur**.  
  
3.  Dans le volet droit, double-cliquez sur **Effectuer des tâches de maintenance sur les volumes**.  
  
4.  Cliquez sur **Ajouter un utilisateur ou un groupe** et ajoutez tout compte d’utilisateur utilisé pour les sauvegardes.  
  
5.  Cliquez sur **Appliquer**, puis fermez toutes les boîtes de dialogue **Stratégie de sécurité locale** .  
  
## <a name="to-turn-instant-file-initialization-on-or-off"></a>Pour activer ou désactiver les initialisation instantanée des fichiers  
  
1.  Lancez le Gestionnaire de Configuration. Pour plus d’informations, consultez [lance le Gestionnaire de Configuration &#40; Système de plateforme Analytique &#41; ](launch-the-configuration-manager.md).  
  
2.  Dans le volet gauche du Gestionnaire de Configuration, cliquez sur **initialisation instantanée des fichiers**.  
  
3.  Pour activer l’initialisation instantanée de fichiers, activez la case à côté **activer initialisation instantanée des fichiers sur tous les nœuds**. Pour désactiver l’initialisation instantanée des fichiers, désactivez la case à côté **activer initialisation instantanée des fichiers sur tous les nœuds**.  
  
    > [!WARNING]  
    > Lorsque vous désactivez l’initialisation instantanée des fichiers, la considération de sécurité décrite ci-dessus pour la fonctionnalité peut s’appliquent toujours aux fichiers supprimés pendant l’initialisation instantanée des fichiers a été activée.  
  
4.  Cliquez sur **Appliquer**. La modification sera propagée via les instances de SQL Server sur SQL Server PDW la prochaine fois que les services d’application sont redémarrés. Pour redémarrer les services d’application maintenant, consultez [PDW l’état des Services &#40; Système de plateforme Analytique &#41; ](pdw-services-status.md).  
  
5.  Vous souhaiterez peut-être Répétez les étapes décrites ci-dessus en tant que **ajouter une autorisation pour le compte de sauvegarde** pour supprimer la **effectuer les tâches de maintenance de volume** autorisation.  
  
![Initialisation de fichiers DWConfig Appliance PDW instantanée](./media/instant-file-initialization-configuration/SQL_Server_PDW_DWConfig_ApplPDWInstant.png "SQL_Server_PDW_DWConfig_ApplPDWInstant")  
  
Pour plus d’informations sur l’initialisation instantanée des fichiers, consultez [initialisation instantanée des fichiers](http://technet.microsoft.com/en-us/library/ms175935(v=SQL.105).aspx).  
  
