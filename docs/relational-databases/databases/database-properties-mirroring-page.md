---
title: Propriétés de la base de données (page Mise en miroir) | Microsoft Docs
ms.custom: ''
ms.date: 08/25/2016
ms.prod: sql
ms.prod_service: database-engine
ms.component: databases
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.swb.databaseproperties.mirroring.f1
ms.assetid: 5bdcd20f-532d-4ee6-b2c7-18dbb7584a87
caps.latest.revision: 86
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: a14e748959ca5553dc6a8e2af6d26700ab418e7d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="database-properties-mirroring-page"></a>Propriétés de la base de données (page Mise en miroir)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Accédez à cette page depuis la base de données principale et utilisez-la pour configurer et modifier les propriétés de la mise en miroir de bases de données pour une base de données. Utilisez-la également pour lancer Configurer l'Assistant Sécurité de mise en miroir de bases de données, afin d'afficher l'état d'une session de mise en miroir et de suspendre ou supprimer une session de mise en miroir de bases de données.  
  
> **IMPORTANT !** La sécurité doit être configurée avant de démarrer la mise en miroir. Si la mise en miroir n'a pas commencé, vous devez la lancer en utilisant l'Assistant. Les zones de texte de la page **Mise en miroir** restent désactivées jusqu’à la fin de l’Assistant.  
  
 **Configurer la mise en miroir de bases de données à l’aide de SQL Server Management Studio**  
  
-   [Établir une session de mise en miroir de bases de données au moyen de l’authentification Windows &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md)  
  
## <a name="options"></a>Options  
 **Configurer la sécurité**  
 Cliquez sur ce bouton pour démarrer **Configurer l’Assistant Sécurité de mise en miroir de bases de données**.  
  
 Si l'Assistant est exécuté avec succès, l'action entreprise dépend du fait que la mise en miroir a déjà commencé ou non, comme suit :  
  
|||  
|-|-|  
|Si la mise en miroir n'a pas commencé.|La page de propriétés met en cache ces informations de connexion, ainsi qu'une valeur indiquant si la propriété de partenaire de la base de données miroir est définie.<br /><br /> À la fin de l'Assistant, vous êtes invité à démarrer la mise en miroir de bases de données en utilisant les adresses réseau et le mode d'opération du serveur par défaut. Si vous souhaitez modifier les adresses ou le mode d’opération, cliquez sur **Ne pas démarrer la mise en miroir**.|  
|Si la mise en miroir a commencé.|Si le serveur témoin a été modifié dans l'Assistant, il est défini en conséquence.|  
  
 **Adresses réseau du serveur**  
 Une option équivalente existe pour chacune des instances de serveur : **Principal**, **Miroir**et **Témoin**.  
  
 Les adresses réseau du serveur des instances de serveurs sont spécifiées automatiquement lorsque vous terminez Configurer l'Assistant Sécurité de mise en miroir de bases de données. Une fois l'Assistant terminé, vous pouvez si nécessaire modifier les adresses réseau manuellement.  
  
 L'adresse réseau du serveur présente la syntaxe de base suivante :  
  
 TCP**://***fully_qualified_domain_name***:***port*  
  
 où  
  
-   *nom_de_domaine_complet* est le serveur sur lequel l’instance de serveur se trouve.  
  
-   *port* est le port attribué au point de terminaison de mise en miroir de bases de données de l’instance de serveur.  
  
     Pour prendre part à la mise en miroir de bases de données, un serveur requiert un point de terminaison de mise en miroir de bases de données. Si vous utilisez Configurer l'Assistant Sécurité de mise en miroir de bases de données pour établir la première session de mise en miroir d'une instance de serveur, l'Assistant crée automatiquement le point de terminaison et le configure pour qu'il utilise l'authentification Windows. Pour plus d’informations sur l’utilisation de l’Assistant avec l’authentification basée sur les certificats, consultez [Établir une session de mise en miroir de bases de données au moyen de l’authentification Windows &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md).  
  
    >**IMPORTANT**  Chaque instance de serveur requiert un seul point de terminaison de mise en miroir de bases de données, quel que soit le nombre de sessions de mise en miroir à prendre en charge.  
  
 Par exemple, pour une instance de serveur d'un système informatique nommé `DBSERVER9` , dont le point de terminaison utilise le port `7022`, l'adresse réseau peut être :  
  
```  
TCP://DBSERVER9.COMPANYINFO.ADVENTURE-WORKS.COM:7022  
```  
  
 Pour plus d’informations, consultez [Spécifier une adresse réseau de serveur &#40;mise en miroir de bases de données&#41;](../../database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md).  
  
> **REMARQUE :** Au cours d’une session de mise en miroir de bases de données, les instances du principal et du serveur miroir ne peuvent pas être modifiées ; l’instance de serveur témoin, toutefois, peut être modifiée au cours d’une session. Pour plus d'informations, consultez la section « Remarques » plus loin dans cette rubrique.  
  
 **Démarrer la mise en miroir**  
 Cliquez ici pour commencer la mise en miroir, lorsque toutes les conditions suivantes sont respectées :  
  
-   La base de données miroir doit exister.  
  
     Avant de démarrer la mise en miroir, la base de données miroir doit avoir été créée par la restauration WITH NORECOVERY d'une sauvegarde complète récente et, éventuellement, de sauvegardes de journaux de la base de données principale sur le serveur miroir. Pour plus d’informations, consultez [Préparer une base de données miroir pour la mise en miroir &#40;SQL Server&#41;](../../database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md).  
  
-   Les adresses TCP des instances de principal et de serveur miroir sont déjà spécifiées (dans la section **Adresses réseau du serveur**).  
  
-   Si le mode d'opération est défini sur Haute sécurité avec basculement automatique (synchrone), l'adresse TCP de l'instance de serveur miroir est également spécifiée.  
  
-   La sécurité a été correctement configurée.  
  
 Cliquez sur **Démarrer la mise en miroir** pour initier la session. Le moteur de base de données tente de se connecter automatiquement au serveur partenaire de mise en miroir pour vérifier que le serveur miroir est correctement configuré et commencer la session de mise en miroir. Si la mise en miroir peut être lancée, un travail est créé pour surveiller la base de données.  
  
 **Suspendre** ou **Reprendre**  
 Au cours d’une session de mise en miroir de bases de données, cliquez sur **Suspendre** pour interrompre la session. Vous êtes invité à confirmer l'opération ; si vous cliquez sur **Oui**, la session est suspendue et le bouton devient **Reprendre**. Pour reprendre la session, cliquez sur **Reprendre**.  
  
 Pour plus d’informations sur les effets de la suspension d’une session, consultez [Suspension et reprise de la mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/pausing-and-resuming-database-mirroring-sql-server.md).  
  
> **IMPORTANT** Après un service forcé, lorsque le serveur principal d'origine se reconnecte, la mise en miroir est suspendue. La reprise de la mise en miroir dans cette situation peut entraîner des pertes de données sur le serveur principal d'origine. Pour plus d’informations sur la gestion des problèmes éventuels de perte de données, consultez [Basculement de rôle durant une session de mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/role-switching-during-a-database-mirroring-session-sql-server.md).  
  
 **Suppression de la mise en miroir**  
 Sur l'instance de serveur principal, cliquez pour interrompre la session et supprimer la configuration de mise en miroir des bases de données. Un message de confirmation s’affiche. Si vous cliquez sur **Oui**, la session s’arrête et la mise en miroir est supprimée. Pour plus d’informations sur l’impact de la suppression d’une mise en miroir de bases de données, consultez [Suppression d’une mise en miroir des bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/removing-database-mirroring-sql-server.md).  
  
> **REMARQUE :** S’il s’agit de la seule base de données mise en miroir sur l’instance de serveur, le travail de surveillance est supprimé.  
  
 **Basculement**  
 Cliquez sur ce bouton pour basculer manuellement de la base de données principale vers la base de données miroir.  
  
> **REMARQUE :** Si la session de mise en miroir est exécutée en mode haute performance, le basculement manuel n’est pas pris en charge. Pour effectuer un basculement manuel, vous devez d’abord définir le mode d’opération sur **Haute sécurité sans basculement automatique (synchrone)**. Une fois le basculement terminé, vous pouvez restaurer le mode **Haute performance (asynchrone)** sur l’instance de serveur principal.  
  
 Vous êtes invité à confirmer l'opération. Si vous cliquez sur **Oui**, une tentative de basculement est effectuée. Le serveur principal commence par essayer de se connecter au serveur miroir à l'aide de l'authentification Windows. Si l’authentification Windows ne fonctionne pas, le serveur principal affiche la boîte de dialogue **Se connecter au serveur** . Si le serveur miroir utilise l’authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , sélectionnez **Authentification SQL Server** dans la zone **Authentification** . Dans la zone de texte **Connexion** , spécifiez le compte de connexion à utiliser pour se connecter sur le serveur miroir puis, dans la zone de texte **Mot de passe** , spécifiez le mot de passe de ce compte.  
  
 Si le basculement réussit, la boîte de dialogue **Propriétés de la base de données** se ferme. Les rôles du principal et du serveur miroir sont échangés : l'ancienne base de données miroir devient la base de données principale et inversement. Notez que la boîte de dialogue **Propriétés de la base de données** devient immédiatement inaccessible sur l’ancienne base de données principale, car cette dernière est devenue la base de données miroir ; cette boîte de dialogue devient accessible sur la nouvelle base de données principale après le basculement.  
  
 Si le basculement échoue, un message d'erreur s'affiche et la boîte de dialogue reste ouverte.  
  
> **IMPORTANT** Si vous cliquez sur **Basculer** après avoir modifié des propriétés dans la boîte de dialogue **Propriétés de la base de données** , ces modifications sont perdues. Pour enregistrer les modifications apportées, répondez **Non** à l’invite de confirmation et cliquez sur **OK** pour enregistrer les modifications. Ensuite, ouvrez de nouveau la boîte de dialogue de propriétés de la base de données et cliquez sur **Basculer**.  
  
 **Mode d'opération**  
 Si vous le souhaitez, vous pouvez modifier le mode d'opération. La disponibilité de certains modes d'opération dépend du fait que vous ayez spécifié ou non une adresse TCP pour un témoin. Les options disponibles sont les suivantes :  
  
|Option|Témoin ?|Explication|  
|------------|--------------|-----------------|  
|**Haute performance (asynchrone)**|Nul (s'il existe, non utilisé mais la session requiert un quorum)|Pour optimiser les performances, la base de données miroir reste toujours en léger décalage par rapport à la base de données principale, sans jamais complètement le rattraper. Toutefois, l'écart entre les bases de données est généralement faible. La perte d'un partenaire a les effets suivants :<br /><br /> Si l'instance de serveur miroir devient non disponible, le principal continue.<br /><br /> Si l'instance de serveur principal n'est plus disponible, le serveur miroir est arrêté. Mais si la session n'a pas de témoin (comme recommandé), ou que le témoin est connecté au serveur miroir, ce dernier reste accessible en veille ; le propriétaire de la base de données peut forcer le service vers l'instance de serveur miroir (avec perte de données éventuelle).|  
|**Haute sécurité sans basculement automatique (synchrone)**|non|Ce mode garantit que toutes les transactions validées sont écrites sur disque sur le serveur miroir.<br /><br /> Le basculement manuel est possible si les partenaires sont connectés entre eux.<br /><br /> La perte d'un partenaire a les effets suivants :<br /><br /> Si l'instance de serveur miroir devient non disponible, le principal continue.<br /><br /> Si l'instance de serveur principal n'est plus disponible, l'instance miroir s'arrête mais reste disponible en veille ; le propriétaire de la base de données peut forcer le service à l'instance de serveur miroir (avec perte de données éventuelle).|  
|**Haute sécurité avec basculement automatique (synchrone)**|Oui (requis)|Disponibilité optimisée par l'inclusion d'une instance de serveur témoin pour prendre en charge le basculement automatique. Notez que vous pouvez sélectionner l’option **Haute sécurité avec basculement automatique (synchrone)** seulement si vous avez spécifié au préalable une adresse de serveur témoin.<br /><br /> Le basculement manuel est toujours possible lorsque les partenaires sont connectés entre eux.<br /><br /> **\*\* Important \*\*** Si le témoin est déconnecté, les partenaires doivent être connectés entre eux pour que la base de données soit disponible. Pour plus d’informations, consultez [Quorum : effets d’un témoin sur la disponibilité de la base de données &#40;mise en miroir de bases de données&#41;](../../database-engine/database-mirroring/quorum-how-a-witness-affects-database-availability-database-mirroring.md).<br /><br /> Dans les modes d'opération synchrones, l'écriture de toutes les transactions validées sur le serveur miroir est garantie. En présence d’un témoin, la conséquence de la perte d’un partenaire est la suivante :<br /><br /> Si l'instance de serveur principal devient non disponible, un basculement automatique se produit. L'instance de serveur miroir prend le rôle de principal et propose sa base de données comme base de données principale.<br /><br /> Si l'instance de serveur miroir devient non disponible, le principal continue.<br /><br /> <br /><br /> Pour plus d'informations, voir [Database Mirroring Operating Modes](../../database-engine/database-mirroring/database-mirroring-operating-modes.md).|  
  
 Une fois que la mise en miroir a démarré, vous pouvez changer le mode d'opération et enregistrer la modification en cliquant sur **OK**.  
  
 Pour plus d’informations sur les modes d’opération, consultez [Modes de fonctionnement de la mise en miroir de bases de données](../../database-engine/database-mirroring/database-mirroring-operating-modes.md).  
  
 **État**  
 Une fois que la mise en miroir a démarré, le panneau **État** affiche l’état de la session de mise en miroir de bases de données tel que lorsque vous avez sélectionné la page **Mise en miroir** . Pour mettre à jour le panneau **État** , cliquez sur le bouton **Actualiser** . Les états possibles sont :  
  
|États|Explication|  
|------------|-----------------|  
|**Cette base de données n'est pas configurée pour la mise en miroir**|Aucune session de mise en miroir de base de données n’existe et aucune activité n’est à signaler dans la page **Mise en miroir** .|  
|**Suspendu**|La base de données principale est disponible mais n'envoie pas de journaux au serveur miroir.|  
|**Aucune connexion**|L'instance de serveur principal ne peut pas se connecter à son partenaire.|  
|**Synchronisation**|Le contenu de la base de données en miroir est décalé par rapport à celui de la base de données principale. L'instance de serveur principal envoie des enregistrements de journal à l'instance de serveur miroir, laquelle applique les modifications à la base de données miroir pour la restaurer par progression.<br /><br /> Lors du démarrage d'une session de mise en miroir de bases de données, les bases de données miroir et principale se trouvent dans cet état.|  
|**Basculement**|Sur l'instance de serveur principal, un basculement manuel (basculement de rôle) a commencé et le serveur passe actuellement par le rôle de miroir. Dans cet état, les connexions utilisateur à la base de données principale sont rapidement interrompues et la base de données adopte le rôle de miroir dans l'instant qui suit.|  
|**Synchronisé**|Lorsque le serveur miroir a rattrapé suffisamment de retard par rapport au serveur principal, l'état de la base de données devient **Synchronisé**. La base de données reste dans cet état aussi longtemps que le serveur principal continue d'envoyer des modifications au serveur miroir et que le serveur miroir continue d'appliquer les modifications à la base de données miroir.<br /><br /> En mode haute sécurité, le basculement est possible sans perte de données.<br /><br /> En mode haute performance, la perte de données peut se produire, même si l’état est **Synchronisé**.|  
  
 Pour plus d’informations, consultez [États de la mise en miroir &#40;SQL Server&#41;](../../database-engine/database-mirroring/mirroring-states-sql-server.md).  
  
 **Actualiser**  
 Cliquez ici pour mettre à jour la zone **État** .  
  
## <a name="remarks"></a>Notes   
 Si vous n’êtes pas familiarisé avec la mise en miroir de base de données, consultez [Mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md).  
  
### <a name="adding-a-witness-to-an-existing-session"></a>Ajout d'un témoin à une session existante  
 Vous pouvez ajouter un témoin à une session existante ou remplacer un témoin existant. Si vous connaissez l’adresse réseau de serveur du témoin, vous pouvez renseigner le champ **Témoin** manuellement. Dans le cas contraire, utilisez Configurer l'Assistant Sécurité de mise en miroir de bases de données pour configurer le témoin. Une fois le champ d’adresse renseigné, assurez-vous que l’option **Haute sécurité avec basculement automatique (synchrone)** est sélectionnée.  
  
 Après avoir configuré un nouveau témoin, vous devez cliquer sur **OK** pour l’ajouter à la session de mise en miroir.  
  
 **Pour ajouter un témoin lors de l'utilisation de l'authentification Windows**  
  
 [Ajouter ou remplacer un témoin de mise en miroir de bases de données &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/add-or-replace-a-database-mirroring-witness-sql-server-management-studio.md)  
  
### <a name="removing-a-witness"></a>Suppression d'un témoin  
 Pour supprimer un témoin, supprimez du champ **Témoin** son adresse réseau du serveur. Si vous passez du mode de sécurité élevée avec basculement automatique au mode haute performance, le champ **Témoin** est automatiquement supprimé.  
  
 Une fois que le témoin a été supprimé, vous devez cliquer sur **OK** pour le supprimer de la session de mise en miroir.  
  
### <a name="monitoring-database-mirroring"></a>Surveillance de la mise en miroir de bases de données  
 Pour surveiller les bases de données mises en miroir sur une instance de serveur, vous pouvez utiliser le moniteur de mise en miroir de bases de données ou la procédure stockée système sp_dbmmonitorresults.  
  
 **Pour surveiller des bases de données mises en miroir**  
  
-   [Démarrer le moniteur de mise en miroir de bases de données &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-database-mirroring-monitor-sql-server-management-studio.md)  
  
-   [sp_dbmmonitorresults &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbmmonitorresults-transact-sql.md)  
  
 Pour plus d’informations, consultez [Surveillance de la mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/monitoring-database-mirroring-sql-server.md).  
  
##  <a name="RelatedTasks"></a> Tâches associées  
  
-   [Spécifier une adresse réseau de serveur &#40;mise en miroir de bases de données&#41;](../../database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md)  
  
-   [Établir une session de mise en miroir de bases de données au moyen de l’authentification Windows &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md)  
  
-   [Démarrer le moniteur de mise en miroir de bases de données &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/start-database-mirroring-monitor-sql-server-management-studio.md)  
  
## <a name="see-also"></a> Voir aussi  
 [Sécurité du transport de la mise en miroir de bases de données et des groupes de disponibilité Always On &#40;SQL Server&#41;](../../database-engine/database-mirroring/transport-security-database-mirroring-always-on-availability.md)   
 [Basculement de rôle durant une session de mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/role-switching-during-a-database-mirroring-session-sql-server.md)   
 [Surveillance de la mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/monitoring-database-mirroring-sql-server.md)   
 [Mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md)   
 [Suspension et reprise de la mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/pausing-and-resuming-database-mirroring-sql-server.md)   
 [Suppression d’une mise en miroir des bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/removing-database-mirroring-sql-server.md)   
 [Témoin de mise en miroir de base de données](../../database-engine/database-mirroring/database-mirroring-witness.md)  
  
  
