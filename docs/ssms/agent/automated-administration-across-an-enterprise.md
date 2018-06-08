---
title: Administration automatisée à l’échelle d’une entreprise | Microsoft Docs
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
- enterprise automatic administration [SQL Server]
- multiserver administration [SQL Server]
- SQL Server Agent jobs, multiserver administration
- jobs [SQL Server Agent], multiserver administration
- master servers [SQL Server], about master servers
- target servers [SQL Server], about target servers
- master servers [SQL Server]
- multiple instances of SQL Server
- target servers [SQL Server]
ms.assetid: 44d8365b-42bd-4955-b5b2-74a8a9f4a75f
caps.latest.revision: 5
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: = azuresqldb-mi-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: f0245dbc2322c5a0e19fcd18eb5b213422403094
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="automated-administration-across-an-enterprise"></a>Administration automatisée à l'échelle d'une entreprise
[!INCLUDE[appliesto-ss-asdbmi-xxxx-xxx-md](../../includes/appliesto-ss-asdbmi-xxxx-xxx-md.md)]

> [!IMPORTANT]  
> Dans [Azure SQL Database Managed Instance](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance), la plupart des fonctionnalités SQL Server Agent sont prises en charge. Pour plus d’informations, consultez [Différences T-SQL entre Azure SQL Database Managed Instance et SQL Server](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Le fait d’automatiser l’administration sur plusieurs instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] est appelé *administration multiserveur*. Utilisez l'administration multiserveur dans les cas suivants :  
  
-   Gérez deux serveurs ou plus ;  
  
-   Planifiez les flux d'informations entre les serveurs de l'entreprise pour constituer un Data Warehouse.  
  
Pour tirer parti d'une administration multiserveur, vous devez disposer d'au moins un serveur maître et d’au moins un serveur cible. Un serveur maître distribue les travaux aux serveurs cibles et reçoit les événements de ces derniers. Un serveur maître stocke également la copie centrale des définitions de travaux pour les travaux exécutés sur des serveurs cibles. Les serveurs cibles se connectent régulièrement au serveur maître pour mettre à jour leur liste des travaux planifiés. Si un nouveau travail se trouve sur le serveur maître, le serveur cible le télécharge. Une fois que le serveur cible a terminé le travail, il se reconnecte au serveur maître et rend compte de l'état du travail. Notez que votre définition de travail doit être identique lors de l’exécution de toute activité liée aux bases de données.  
  
L'illustration suivante décrit la relation entre serveurs maîtres et cibles :  
  
![Configuration de l’administration multiserveur](../../ssms/agent/media/multisvr.gif "Configuration de l’administration multiserveur")  
  
Si vous administrez les serveurs départementaux d'une grande société, vous pouvez définir les éléments suivants :  
  
-   un travail de sauvegarde en plusieurs étapes ;  
  
-   les opérateurs à avertir en cas d'échec de sauvegarde ;  
  
-   un programme d'exécution pour le travail de sauvegarde.  
  
Écrivez ce travail de sauvegarde une seule fois sur le serveur maître, puis inscrivez chaque serveur du service comme serveur cible dans le serveur maître. Dès lors qu'ils sont inscrits, tous les serveurs départementaux exécutent le même travail de sauvegarde, alors que vous n'avez défini le travail qu'une seule fois.  
  
> [!NOTE]  
> Les fonctionnalités d'administration multiserveur s'adressent aux membres du rôle sysadmin. Cependant, un membre du rôle sysadmin sur le serveur cible ne peut pas modifier les opérations exécutées sur le serveur cible par le serveur maître. Cette mesure de sécurité empêche la suppression accidentelle des étapes du travail et l’interruption des opérations sur le serveur cible.  
  
## <a name="in-this-section"></a>Dans cette section  
[Créer un environnement multi-serveur](../../ssms/agent/create-a-multiserver-environment.md)  
Contient des informations sur la création et la gestion des serveurs maîtres et cibles.  
  
[Choisir le compte de service SQL Server Agent correct pour les environnements multiserveurs](../../ssms/agent/choose-the-right-sql-server-agent-service-account-for-multiserver-environments.md)  
Contient des informations sur la manière dont l'utilisation de comptes Windows non administratifs ou du compte système local pour le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent peut affecter les environnements multiserveurs.  
  
[Définir des options de chiffrement sur des serveurs cibles](../../ssms/agent/set-encryption-options-on-target-servers.md)  
Contient des informations sur la définition de la sous-clé de Registre[!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Agent MsxEncryptChannelOptions sur les serveurs cibles.  
  
[Gérer des travaux à l'échelle d'une entreprise](../../ssms/agent/manage-jobs-across-an-enterprise.md)  
Contient des informations concernant la vérification de l'état des travaux, la modification des serveurs cibles pour les travaux, la synchronisation des horloges des serveurs cibles et l'interrogation des serveurs maîtres quant à l'état des travaux en cours.  
  
[Résoudre les problèmes liés aux travaux multiserveurs qui utilisent des proxys](../../ssms/agent/troubleshoot-multiserver-jobs-that-use-proxies.md)  
Contient des informations relatives au dépannage des travaux multiserveurs qui utilisent des proxys qui échouent.  
  
[Interroger des serveurs](../../ssms/agent/poll-servers.md)  
Contient des informations sur la façon de configurer de manière implicite et explicite les serveurs cibles, de sorte qu'ils interrogent les serveurs maîtres pour la synchronisation des informations sur les travaux.  
  
[Gérer les événements](../../ssms/agent/manage-events.md)  
Contient des informations concernant le transfert d'événements des serveurs cibles vers les serveurs maîtres.  
  
[Paramétrer l'administration automatisée dans une entreprise](../../ssms/agent/tune-automated-administration-across-an-enterprise.md)  
Contient des informations décrivant la façon dont l'administration automatisée au sein d'un environnement multiserveur exploite les fonctionnalités d'autoconfiguration de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)].  
  
## <a name="see-also"></a> Voir aussi  
[Rubriques relatives à la compatibilité descendante pour l’installation du moteur de base de données SQL Server](http://msdn.microsoft.com/en-us/10de5ec6-d3cf-42ef-aa62-1bdf3fbde841)  
[Inscription des serveurs](http://msdn.microsoft.com/en-us/c2a2513e-fa09-419c-99e7-a12d57c5a0db)  
[sp_add_targetservergroup](http://msdn.microsoft.com/en-us/acb69343-d766-46ff-b771-0c7655c5231a)  
[sp_delete_targetserver](http://msdn.microsoft.com/en-us/cc438701-ad91-419d-9f23-ebc4c548c700)  
[sp_delete_targetservergroup](http://msdn.microsoft.com/en-us/d8dd838e-64aa-419f-9ccb-ff04908cf3e4)  
[_sp_help_downloadlist](http://msdn.microsoft.com/en-us/745b265b-86e8-4399-b928-c6969ca1a2c8)  
[sp_help_jobserver](http://msdn.microsoft.com/en-us/57971787-f9f5-4199-9f64-c2b61a308906)  
[sp_help_targetservergroup](http://msdn.microsoft.com/en-us/ec3a4a68-b591-431c-9518-053ede522d0c)  
[sp_resync_targetserver](http://msdn.microsoft.com/en-us/40e44df7-d3e3-44ee-b149-08aba629a21f)  
[sp_update_targetservergroup](http://msdn.microsoft.com/en-us/4ac65ed6-e07e-40e4-a282-13bfd92dfa41)  
[sysjobservers](http://msdn.microsoft.com/en-us/9abcc20f-a421-4591-affb-62674d04575e)  
[syslogins](http://msdn.microsoft.com/en-us/4cb34f17-a4bb-469f-a218-71f074e6308f)  
[systargetservers](http://msdn.microsoft.com/en-us/479d1314-be37-4d19-ac9c-419fc9110e53)  
  
