---
title: server memory (options de configuration du serveur) | Microsoft Docs
ms.custom: ''
ms.date: 11/27/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.suite: sql
ms.technology: configuration
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- Virtual Memory Manager
- max server memory option
- virtual memory [SQL Server]
- VMM
- server memory options [SQL Server]
- servers [SQL Server], memory
- buffer pools [SQL Server]
- min server memory option
- manual memory options [SQL Server]
- memory [SQL Server], servers
ms.assetid: 29ce373e-18f8-46ff-aea6-15bbb10fb9c2
caps.latest.revision: 78
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 3544a4530c1650d02952c750d82bb9d51e2d6d50
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="server-memory-server-configuration-options"></a>server memory (options de configuration du serveur)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

Utilisez les deux options de mémoire du serveur, **min server memory** et **max server memory**, pour reconfigurer la quantité de mémoire (en mégaoctets) gérée par le Gestionnaire de mémoire de SQL Server pour un processus SQL Server utilisé par une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
Le paramètre par défaut de l’option **min server memory** est 0 et celui de l’option **max server memory** est 2 147 483 647 mégaoctets (Mo). Par défaut, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut modifier dynamiquement sa configuration mémoire en fonction des ressources système disponibles. Pour plus d’informations, consultez [Gestion de la mémoire dynamique](../../relational-databases/memory-management-architecture-guide.md#dynamic-memory-management). 

La taille mémoire minimale autorisée pour **max server memory** est de 128 Mo.
  
> [!IMPORTANT]  
> Si vous affectez à **max server memory** une valeur trop élevée, une instance unique de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut être contrainte de rivaliser avec d’autres instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] hébergées sur le même hôte pour obtenir de la mémoire. Toutefois, une valeur trop faible peut entraîner des problèmes importants liés aux performances et à la sollicitation de la mémoire. Si vous affectez à **max server memory** la valeur minimale, vous pouvez même empêcher [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de démarrer. Si vous ne pouvez plus démarrer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] après avoir changé cette option, démarrez-le au moyen de l’option de démarrage ***–f*** et restaurez **max server memory** à sa valeur antérieure. Pour plus d’informations, consultez [Options de démarrage du service moteur de base de données](../../database-engine/configure-windows/database-engine-service-startup-options.md).  
    
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut utiliser dynamiquement la mémoire ; cependant, vous pouvez configurer manuellement les options de mémoire et limiter la mémoire à laquelle [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut accéder. Avant de définir la mémoire allouée à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], déterminez la valeur adaptée pour la mémoire : pour cela, vous devez soustraire de la mémoire physique totale la mémoire exigée par le système d’exploitation, par les allocations de mémoire non contrôlées à l’aide du paramètre max_server_memory et par toute autre instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (ainsi que par d’autres systèmes si l’ordinateur n’est pas totalement dédié à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]). Cette différence représente la mémoire maximale que vous pouvez allouer à l’instance actuelle de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
 
## <a name="setting-the-memory-options-manually"></a>Paramétrage manuel des options de mémoire  
Vous pouvez définir les options de serveur **min server memory** et **max server memory** pour couvrir une plage de valeurs de mémoire. Cette méthode est utile pour les administrateurs système ou de bases de données qui souhaitent configurer une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en accord avec la mémoire exigée par d’autres applications ou d’autres instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui s’exécutent sur le même hôte.

> [!NOTE]
> **min server memory** et **max server memory** sont des options avancées. Si vous utilisez la procédure stockée système **sp_configure** pour changer ces paramètres, vous ne pouvez les modifier que si l’option **show advanced options** a la valeur 1. Ces paramètres entrent immédiatement en vigueur, sans redémarrage du serveur.  
  
<a name="min_server_memory"></a> Utilisez **min_server_memory** pour garantir une quantité minimale de mémoire accessible au Gestionnaire de mémoire de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n'alloue pas immédiatement la mémoire spécifiée dans **min server memory** au démarrage. Néanmoins, lorsque l’utilisation de la mémoire atteint cette valeur en raison de la charge client, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne peut libérer de la mémoire à moins que la valeur **min server memory** ne soit réduite. Par exemple, si plusieurs instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peuvent exister simultanément sur le même ordinateur hôte, définissez min_server_memory plutôt que max_server_memory afin de réserver de la mémoire pour une instance. La définition d’une valeur min_server_memory est essentielle dans un environnement virtualisé. Elle permet en effet de garantir que la sollicitation de la mémoire de l’hôte sous-jacent ne tente pas de libérer, dans le pool de tampons d’une machine virtuelle [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] invitée, une quantité de mémoire supérieure à celle nécessaire pour obtenir des performances acceptables.
 
> [!NOTE]  
> Il n’est pas garanti que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] alloue la mémoire spécifiée dans **min server memory**. Si la charge sur le serveur ne nécessite jamais d’allouer la mémoire spécifiée dans **min server memory**, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s’exécute alors avec moins de mémoire.  
  
<a name="max_server_memory"></a> Utilisez **max_server_memory** pour que le système d’exploitation ne fasse pas l’objet d’une sollicitation de la mémoire préjudiciable. Pour définir la configuration de la mémoire maximale du serveur, surveillez la consommation globale du processus [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] afin de déterminer les besoins en mémoire. Pour obtenir des résultats plus précis pour une instance unique :
 -  Réservez 1 à 4 Go de la mémoire totale du système d’exploitation au système d’exploitation lui-même.
 -  Ensuite, soustrayez l’équivalent des allocations de mémoire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] potentielles en dehors du contrôle **max server memory**, à savoir la ***taille de la pile<sup>1</sup>*, le nombre maximal calculé de threads de worker<sup>2</sup> et le paramètre de démarrage -g<sup>3</sup>*** (ou 256 Mo par défaut si *-g* n’est pas défini). Il doit rester le paramètre max_server_memory pour une installation d’instance unique.
 
<sup>1</sup> Pour plus d’informations sur les tailles de piles de threads par architecture, consultez le [guide d’architecture de gestion de la mémoire](../../relational-databases/memory-management-architecture-guide.md#stacksizes).

<sup>2</sup> Pour plus d’informations sur les threads de worker par défaut calculés pour un nombre donné d’UC avec affinité dans l’hôte actif, consultez la page [Configurer l’option de configuration du serveur max worker threads](../../database-engine/configure-windows/configure-the-max-worker-threads-server-configuration-option.md) dans la documentation.

<sup>3</sup> Pour plus d’informations sur le paramètre de démarrage *-g*, consultez la page [Options de démarrage du service moteur de base de données](../../database-engine/configure-windows/database-engine-service-startup-options.md) dans la documentation.

## <a name="how-to-configure-memory-options-using-includessmanstudiofullincludesssmanstudiofull-mdmd"></a>Comment configurer les options de mémoire à l’aide de[!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]  
Utilisez les deux options de mémoire du serveur, **min server memory** et **max server memory**, pour reconfigurer la quantité de mémoire (en mégaoctets) gérée par le Gestionnaire de mémoire de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Par défaut, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut modifier dynamiquement sa configuration mémoire en fonction des ressources système disponibles.  
  
### <a name="procedure-for-configuring-a-fixed-amount-of-memory-not-recommended"></a>Procédure de configuration d’une quantité de mémoire fixe (non recommandé)  
Pour définir une quantité fixe de mémoire :  
  
1.  Dans l’Explorateur d’objets, cliquez avec le bouton droit sur un serveur et sélectionnez **Propriétés**.  
  
2.  Cliquez sur le nœud **Mémoire** .  
  
3.  Sous **Options mémoire du serveur**, entrez la même quantité pour **Mémoire minimale du serveur** et **Mémoire maximale du serveur**.  
  
     Utilisez les paramètres par défaut pour autoriser [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à modifier ses besoins de mémoire de façon dynamique en fonction des ressources système disponibles. Il est recommandé de définir un paramètre **max server memory**, comme [indiqué ci-dessus](#max_server_memory). 
  
## <a name="lock-pages-in-memory-lpim"></a>Verrouiller les pages en mémoire (LPIM) 
Cette stratégie Windows détermine quels comptes peuvent utiliser un processus destiné à conserver les données en mémoire physique pour éviter leur pagination en mémoire virtuelle sur le disque. Le verrouillage des pages en mémoire peut permettre de conserver sa réactivité au serveur lors de la pagination de la mémoire sur disque. L’option **Verrouiller les pages en mémoire** a la valeur ON dans les instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] éditions Standard et supérieures quand le compte avec les privilèges nécessaires pour exécuter sqlservr.exe dispose du droit d’utilisateur Windows *Verrouiller les pages en mémoire* (LPIM).  
  
Pour désactiver l’option **Verrouiller les pages en mémoire** pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], supprimez le droit d’utilisateur *Verrouiller les pages en mémoire* pour le compte disposant de privilèges pour exécuter le compte de démarrage sqlservr.exe (compte de démarrage de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]).  
 
Cette option n’affecte pas la [gestion de la mémoire dynamique](../../relational-databases/memory-management-architecture-guide.md#dynamic-memory-management) de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Elle peut donc être augmentée ou réduite à la demande d’autres régisseurs de mémoire. Quand vous utilisez le droit d’utilisateur *Verrouiller les pages en mémoire*, il est recommandé de définir une limite supérieure pour **max server memory**, comme [indiqué ci-dessus](#max_server_memory).

> [!IMPORTANT]
> Cette option ne doit être utilisée qu’en cas de nécessité, à savoir s’il y a des raisons de penser que le processus sqlservr est hors page. Dans ce cas, l’erreur 17890, qui ressemble à l’exemple ci-dessous, est signalée dans le journal des erreurs : `A significant part of sql server process memory has been paged out. This may result in a performance degradation. Duration: #### seconds. Working set (KB): ####, committed (KB): ####, memory utilization: ##%.` À compter de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], Standard Edition n’a pas besoin de [l’indicateur de trace 845](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) pour utiliser les pages verrouillées. 
  
### <a name="to-enable-lock-pages-in-memory"></a>Pour activer Verrouiller les pages en mémoire  
Pour activer l’option Verrouiller les pages en mémoire :  
  
1.  Dans le menu **Démarrer** , cliquez sur **Exécuter**. Dans la zone **Ouvrir** , tapez **gpedit.msc**  
  
     La boîte de dialogue **Stratégie de groupe** s'affiche.  
  
2.  Sur la console **Stratégie de groupe** , développez **Configuration de l'ordinateur**, puis **Paramètres Windows**.  
  
3.  Développez **Paramètres de sécurité**, puis **Stratégies locales**.  
  
4.  Sélectionnez le dossier **Attribution des droits utilisateur** .  
  
     Les stratégies s'affichent dans le volet Détails.  
  
5.  Dans le volet, double-cliquez sur **Verrouiller les pages en mémoire**.  
  
6.  Dans la boîte de dialogue **Paramètre de stratégie de sécurité locale**, ajoutez le compte avec les privilèges nécessaires pour exécuter sqlservr.exe (compte de démarrage de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]).  
  
## <a name="running-multiple-instances-of-includessnoversionincludesssnoversion-mdmd"></a>Exécution de plusieurs instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
 Lorsque vous exécutez plusieurs instances de [!INCLUDE[ssDE](../../includes/ssde-md.md)], vous avez le choix entre trois approches pour gérer la mémoire :  
  
-   Utiliser **max server memory** pour contrôler l’utilisation de la mémoire, comme [indiqué ci-dessus](#max_server_memory). Définir les valeurs maximales pour chaque instance, en veillant à ce que le total alloué ne soit pas supérieur à la mémoire physique totale de votre ordinateur. Il se peut que vous souhaitiez attribuer à chaque instance une mémoire proportionnelle à la charge ou à la taille de base de données prévue. Cette solution présente l'avantage qu'au démarrage des nouveaux processus ou des nouvelles instances, ils pourront accéder immédiatement à la mémoire libre. En revanche, cette solution présente l'inconvénient que, si toutes les instances ne sont pas en cours d'exécution, aucune d'entre elles ne pourra utiliser la mémoire libre restante.  
  
-   Utiliser **min server memory** pour contrôler l’utilisation de la mémoire, comme [indiqué ci-dessus](#min_server_memory). Définissez les valeurs minimales pour chaque instance, de telle sorte que leur somme soit inférieure de 1 à 2 Go à la mémoire physique totale de votre machine. Une fois encore, vous pouvez établir ces valeurs minimales de façon proportionnelle à la charge prévue de cette instance. Cette solution présente l'avantage que si toutes les instances ne sont pas en cours d'exécution au même instant, celles qui le sont peuvent utiliser la mémoire libre restante. Elle est également utile quand un autre processus gourmand en mémoire est présent sur l'ordinateur, car elle garantit que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] bénéficie au moins d'une quantité de mémoire acceptable. Le désagrément est que, lorsqu'une nouvelle instance (ou un autre processus) démarre, il se peut que les instances en cours d'exécution mettent un certain temps à libérer de la mémoire, notamment si elles doivent à cette fin réécrire les pages modifiées sur leurs bases de données.  
  
-   Ne rien faire (déconseillé). Les premières instances présentées avec une charge de travail tendent à allouer la totalité de la mémoire. Les instances inactives ou les instances ayant démarré ultérieurement peuvent finir par ne disposer que d'une quantité de mémoire minime. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n'effectue aucune tentative pour équilibrer l'utilisation de la mémoire entre les instances. Cependant, toutes les instances répondent aux signaux de Windows Notification Memory pour ajuster la taille de leur occupation mémoire. Windows n'équilibre pas la mémoire entre les applications avec l'API Memory Notification. Il fournit simplement un commentaire global quant à la disponibilité de la mémoire sur le système.  
  
 Comme vous pouvez modifier ces paramètres sans redémarrer les instances, vous pouvez sans peine procéder à des essais pour trouver les valeurs qui conviennent le mieux à votre modèle d'utilisation.  
  
## <a name="providing-the-maximum-amount-of-memory-to-sql-server"></a>Apport de la quantité maximale de mémoire à SQL Server  
La mémoire peut être configurée jusqu’à la limite de l’espace d’adressage virtuel utilisé par le processus dans toutes les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations, consultez [Limites de mémoire pour les versions de Windows et de Windows Server](http://msdn.microsoft.com/library/windows/desktop/aa366778(v=vs.85).aspx#physical_memory_limits_windows_server_2016).
  
## <a name="examples"></a>Exemples  
  
### <a name="example-a"></a>Exemple A  
 L'exemple suivant affecte la valeur 4 Go à l'option `max server memory` .  
  
```sql  
sp_configure 'show advanced options', 1;  
GO  
RECONFIGURE;  
GO  
sp_configure 'max server memory', 4096;  
GO  
RECONFIGURE;  
GO  
```  
  
### <a name="example-b-determining-current-memory-allocation"></a>Exemple B. Détermination de l’allocation de mémoire actuelle  
 La requête suivante retourne des informations sur la mémoire allouée actuellement.  
  
```sql  
SELECT 
  physical_memory_in_use_kb/1024 AS sql_physical_memory_in_use_MB, 
    large_page_allocations_kb/1024 AS sql_large_page_allocations_MB, 
    locked_page_allocations_kb/1024 AS sql_locked_page_allocations_MB,
    virtual_address_space_reserved_kb/1024 AS sql_VAS_reserved_MB, 
    virtual_address_space_committed_kb/1024 AS sql_VAS_committed_MB, 
    virtual_address_space_available_kb/1024 AS sql_VAS_available_MB,
    page_fault_count AS sql_page_fault_count,
    memory_utilization_percentage AS sql_memory_utilization_percentage, 
    process_physical_memory_low AS sql_process_physical_memory_low, 
    process_virtual_memory_low AS sql_process_virtual_memory_low
FROM sys.dm_os_process_memory;  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Guide d’architecture de gestion de la mémoire](../../relational-databases/memory-management-architecture-guide.md)   
 [Surveiller et régler les performances](../../relational-databases/performance/monitor-and-tune-for-performance.md)   
 [RECONFIGURE &#40;Transact-SQL&#41;](../../t-sql/language-elements/reconfigure-transact-sql.md)   
 [Options de configuration de serveur &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)   
 [Options de démarrage du service moteur de base de données](../../database-engine/configure-windows/database-engine-service-startup-options.md)   
 [Éditions et fonctionnalités prises en charge de SQL Server 2016](../../sql-server/editions-and-components-of-sql-server-2016.md#Cross-BoxScaleLimits)   
 [Éditions et fonctionnalités prises en charge de SQL Server 2017](../../sql-server/editions-and-components-of-sql-server-2017.md#Cross-BoxScaleLimits)   
 [Éditions et fonctionnalités prises en charge de SQL Server 2017 sur Linux](../../linux/sql-server-linux-editions-and-components-2017.md#Cross-BoxScaleLimits)   
 [Limites de mémoire pour les versions de Windows et de Windows Server](http://msdn.microsoft.com/library/windows/desktop/aa366778(v=vs.85).aspx)
 
