---
title: Initialisation instantanée des fichiers de base de données | Microsoft Docs
ms.custom: ''
ms.date: 01/09/2018
ms.prod: sql
ms.prod_service: database-engine
ms.component: databases
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- initializing files [SQL Server]
- instant file initialization [SQL Server]
- fast file initialization [SQL Server]
- file initialization [SQL Server]
- IFI [SQL Server]
- database instant file initialization [SQL Server]
ms.assetid: 1ad468f5-4f75-480b-aac6-0b01b048bd67
caps.latest.revision: 33
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 0f51ea699c269ddb237f4582d4f368dd76cdc149
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="database-file-initialization"></a>Initialisation des fichiers de base de données
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
Les fichiers de données et les fichiers journaux sont initialisés pour remplacer toutes les données existantes laissées sur le disque par des fichiers précédemment supprimés. Les fichiers de données et les fichiers journaux sont d’abord initialisés en étant remplis avec des zéros quand vous effectuez une des opérations suivantes :  
  
- Créer une base de données.  
- Ajouter des fichiers journaux ou de données à une base de données existante.  
- Augmenter la taille d'un fichier existant (opérations de croissance automatique incluses).  
- Restaurer une base de données ou un groupe de fichiers.  
  
L'initialisation des fichiers augmente le temps d'exécution de ces opérations. En revanche, quand les données sont écrites dans les fichiers pour la première fois, le système d'exploitation n'a pas besoin de remplir ces fichiers à l'aide de zéros.  
  
## <a name="instant-file-initialization-ifi"></a>Initialisation instantanée de fichiers  
Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], les fichiers de données peuvent être initialisés instantanément pour éviter les opérations de remplissage avec des zéros. L’initialisation instantanée des fichiers permet une exécution rapide des opérations mentionnées plus haut. L'initialisation instantanée des fichiers récupère l'espace disque utilisé sans le remplir avec des zéros. À la place, le contenu du disque est remplacé à mesure que de nouvelles données sont écrites dans les fichiers. Les fichiers journaux ne peuvent pas être initialisés instantanément.  
  
> [!NOTE]  
> L’initialisation instantanée des fichiers n’est disponible que sur [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[winxppro](../../includes/winxppro-md.md)] ou [!INCLUDE[winxpsvr](../../includes/winxpsvr-md.md)] (ou versions ultérieures).  

> [!IMPORTANT]
> L’initialisation instantanée de fichiers est disponible seulement pour les fichiers de données. Les fichiers journaux sont toujours remplis de zéros à la création ou lors d’une augmentation de leur taille.
  
L’initialisation instantanée de fichiers est disponible seulement que si l’autorisation *SE_MANAGE_VOLUME_NAME* a été octroyée au compte de démarrage de service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Les membres du groupe Administrateurs Windows disposent de ce droit et peuvent l’attribuer aux autres utilisateurs en les ajoutant à la stratégie de sécurité **Effectuer les tâches de maintenance de volume** .  
  
> [!IMPORTANT]
> L’utilisation de certaines fonctionnalités, comme [Transparent Data Encryption (TDE)](../../relational-databases/security/encryption/transparent-data-encryption.md), peuvent empêcher l’initialisation instantanée de fichiers.  
  
Pour accorder l'autorisation `Perform volume maintenance tasks` à un compte  
  
1.  Sur l’ordinateur où le fichier de sauvegarde doit être créé, ouvrez l’application **Stratégie de sécurité locale** (`secpol.msc`).  
  
2.  Dans le volet gauche, développez **Stratégies locales**, puis cliquez sur **Attribution des droits utilisateur**.  
  
3.  Dans le volet droit, double-cliquez sur **Effectuer des tâches de maintenance sur les volumes**.  
  
4.  Cliquez sur **Ajouter un utilisateur ou un groupe** et ajoutez tout compte d’utilisateur utilisé pour les sauvegardes.  
  
5.  Cliquez sur **Appliquer**, puis fermez toutes les boîtes de dialogue **Stratégie de sécurité locale** .  

> [!NOTE]
> À compter de [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)], vous pouvez accorder cette autorisation au compte de service au moment de l’installation. Si vous effectuez [l’installation depuis une invite de commandes](../../database-engine/install-windows/install-sql-server-from-the-command-prompt.md), ajoutez l’argument /SQLSVCINSTANTFILEINIT ou cochez la case *Accorder le privilège Effectuer une tâche de maintenance en volume au service Moteur de base de données SQL Server* dans [l’Assistant Installation](../../database-engine/install-windows/install-sql-server-from-the-installation-wizard-setup.md).

> [!NOTE]
> À compter de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP4, et de [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] SP1 jusqu’à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], vous pouvez utiliser la colonne *instant_file_initialization_enabled* dans la vue de gestion dynamique [sys.dm_server_services](../../relational-databases/system-dynamic-management-views/sys-dm-server-services-transact-sql.md) pour déterminer si l’initialisation instantanée de fichiers est activée.

## <a name="remarks"></a>Notes 
Si le compte de démarrage de service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dispose de l’autorisation *SE_MANAGE_VOLUME_NAME*, un message d’information similaire au suivant est journalisé dans le journal des erreurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] au démarrage : 

```
Database Instant File Initialization: enabled. For security and performance considerations see the topic 'Database Instant File Initialization' in SQL Server Books Online. This is an informational message only. No user action is required.
```

Si le compte de démarrage de service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **ne dispose pas** de l’autorisation *SE_MANAGE_VOLUME_NAME*, un message d’information similaire au suivant est journalisé dans le journal des erreurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] au démarrage : 

```
Database Instant File Initialization: disabled. For security and performance considerations see the topic 'Database Instant File Initialization' in SQL Server Books Online. This is an informational message only. No user action is required.
```

**S’applique à :** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (à compter de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP4, [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2 et de [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] jusqu’à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)])

## <a name="security-considerations"></a>Considérations relatives à la sécurité  
Quand vous utilisez l’initialisation instantanée de fichiers (IFI), comme le contenu du disque supprimé n’est remplacé qu’au moment où de nouvelles données sont écrites dans les fichiers, il est éventuellement accessible à un principal non autorisé jusqu’à ce que d’autres données soient écrites sur cette zone spécifique du fichier de données. Même si le fichier de base de données est attaché à l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], le risque de divulgation de ces informations est limité par la liste de contrôle d’accès discrétionnaire (DACL, Discretionary Access Control List) du fichier. Cette liste DACL n'autorise l'accès au fichier qu'à l'administrateur local et au compte de service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Cependant, quand le fichier est détaché, un utilisateur ou un service ne bénéficiant pas de l’autorisation *SE_MANAGE_VOLUME_NAME* peut y accéder. Cette situation se présente aussi quand la base de données est sauvegardée : si le fichier de sauvegarde n’est pas protégé par une liste DACL appropriée, le contenu supprimé peut devenir accessible à un utilisateur ou à un service non autorisé.  

Une autre considération est que quand la taille est augmentée avec IFI, un administrateur SQL Server peut potentiellement accéder au contenu de la page brute et voir le contenu précédemment supprimé.

Si les fichiers de base de données sont hébergés sur un réseau de zone de stockage, il est également possible que le réseau de zone de stockage présente toujours les nouvelles pages préinitialisées. Or, laisser le système d’exploitation réinitialiser les pages peut représenter une charge supplémentaire non nécessaire.
 
> [!NOTE]
> Si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est installé dans un environnement physique sécurisé, les gains de performance de l’activation de l’initialisation instantanée de fichiers peuvent primer sur les risques de sécurité et, donc, justifier cette recommandation.
  
Si le risque de divulgation du contenu supprimé constitue un problème, effectuez l’une et/ou l’autre des actions suivantes :  
  
- Assurez-vous toujours que les fichiers de sauvegarde et les fichiers de données détachés possèdent des listes DACL restrictives.  
- Désactivez l’initialisation instantanée des fichiers pour l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en révoquant l’autorisation *SE_MANAGE_VOLUME_NAME* au compte de démarrage du service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. 

> [!IMPORTANT]
> La désactivation de l’initialisation instantanée de fichiers augmente les durées d’allocation des fichiers de données.  
  
> [!NOTE]  
> La désactivation de l'initialisation instantanée des fichiers n'affecte que les fichiers créés ou dont la taille a augmenté après la révocation du droit de l'utilisateur.  
  
## <a name="see-also"></a> Voir aussi  
 [CREATE DATABASE &#40;SQL Server Transact-SQL&#41;](../../t-sql/statements/create-database-sql-server-transact-sql.md)  
  
  
