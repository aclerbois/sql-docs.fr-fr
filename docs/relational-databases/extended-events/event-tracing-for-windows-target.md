---
title: Suivi d’événements pour Windows en cible | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology: xevents
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- event tracing for windows target
- ETW target
- targets [SQL Server extended events], event tracing for windows target
ms.assetid: ca2bb295-b7f6-49c3-91ed-0ad4c39f89d5
caps.latest.revision: 13
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 7f3471f30347decd4a6a7f882aed43c8d1325107
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="event-tracing-for-windows-target"></a>suivi d'événements pour cible Windows
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]

  Avant d'utiliser le suivi d'événements pour Windows (ETW) comme cible, il est recommandé d'avoir une connaissance pratique du Suivi d'événements pour Windows. Le suivi ETW est utilisé conjointement avec les Événements étendus ou en tant que consommateur d'événements des Événements étendus. Les liens externes suivants fournissent un point de départ pour obtenir des informations générales sur le suivi ETW :  
  
-   [Événements Windows](http://go.microsoft.com/fwlink/?LinkId=92380)  
  
-   [Améliorer le débogage et le réglage des performances à l'aide du suivi ETW](http://go.microsoft.com/fwlink/?LinkId=92381)  
  
 La cible du suivi ETW est une cible singleton, bien qu'elle puisse être ajoutée à plusieurs sessions. Si un événement est déclenché sur plusieurs sessions, l'événement sera propagé uniquement à la cible ETW une fois par occurrence de l'événement. Le moteur des Événements étendus est limité à une seule instance par processus.  
  
> [!IMPORTANT]  
>  Pour que la cible du suivi ETW fonctionne, le compte de démarrage du service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit être membre du groupe Utilisateurs du journal de performances.  
  
 La configuration des événements présents dans une session ETW est contrôlée par le processus qui héberge le moteur des Événements étendus. Le moteur contrôle quels événements déclencher et quelles conditions doivent être respectées pour qu'un événement se déclenche.  
  
 Après avoir créé une liaison à une session Événements étendus, ce qui joint la cible ETW pour la première fois pendant la durée de vie d'un processus, la cible ETW ouvre une session ETW unique sur le fournisseur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Si une session ETW existe déjà, la cible ETW obtient une référence à la session existante. Cette session ETW est partagée sur toutes les instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur un ordinateur donné. Cette session ETW reçoit tous les événements à partir des sessions qui possèdent la cible ETW.  
  
 Comme ETW a besoin de fournisseurs pour pouvoir consommer des événements et les transmettre en aval au suivi ETW, tous les packages des Événements étendus sont activés sur la session. Lorsqu'un événement est déclenché, la cible ETW envoie l'événement à la session sur laquelle le fournisseur pour l'événement est activé.  
  
 La cible ETW prend en charge la publication synchrone d'événements sur le thread qui déclenche l'événement. Cependant, la cible ETW ne prend pas en charge la publication asynchrone d'événements.  
  
 La cible ETW ne prend pas en charge le contrôle à partir de contrôleurs ETW externes tels que Logman.exe. Pour produire des traces ETW, une session d'événements doit être créée avec la cible ETW. Pour plus d’informations, consultez [CREATE EVENT SESSION &#40;Transact-SQL&#41;](../../t-sql/statements/create-event-session-transact-sql.md).  
  
> [!NOTE]  
>  L'activation de la cible ETW crée une session ETW nommée XE_DEFAULT_ETW_SESSION. Si une session XE_DEFAULT_ETW_SESSION existe déjà, elle est utilisée sans modifier la moindre propriété de la session existante. La session XE_DEFAULT_ETW_SESSION est partagée entre toutes les instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Après avoir démarré XE_DEFAULT_ETW_SESSION, vous devez l'arrêter à l'aide d'un contrôleur ETW, tel que l'outil Logman. Par exemple, vous pouvez exécuter la commande suivante à l’invite de commandes : **logman stop XE_DEFAULT_ETW_SESSION -ets**.  
  
 Le tableau suivant décrit les options disponibles pour configurer la cible ETW.  
  
|Option|Valeurs autorisées|Description|  
|------------|--------------------|-----------------|  
|default_xe_session_name|Toute chaîne incluant jusqu'à 256 caractères. Cette valeur est facultative.|Nom de la session Événements étendus. Par défaut, il s'agit de la session XE_DEFAULT_ETW_SESSION.|  
|default_etw_session_logfile_path|Toute chaîne incluant jusqu'à 256 caractères. Cette valeur est facultative.|Chemin d'accès du fichier journal pour la session Événements étendus. Par défaut, il s'agit de %TEMP%\ XEEtw.etl.|  
|default_etw_session_logfile_size_mb|Entier quelconque non signé. Cette valeur est facultative.|Taille du fichier journal, en mégaoctets (Mo), de la session Événements étendus. La valeur par défaut est 20 Mo.|  
|default_etw_session_buffer_size_kb|Entier quelconque non signé. Cette valeur est facultative.|Taille des tampons en mémoire, en kilo-octets (Ko), pour la session Événements étendus. La valeur par défaut est 128 Ko.|  
|retries|Entier quelconque non signé.|Nombre de nouvelles tentatives de publication de l'événement dans le sous-système ETW avant la suppression de l'événement. La valeur par défaut est 0.|  
  
 La configuration de ces paramètres est facultative. La cible ETW utilise les valeurs par défaut de ces paramètres.  
  
 La cible ETW est chargée des opérations suivantes :  
  
-   Création de la session ETW par défaut.  
  
-   Enregistrement de tous les packages des Événements étendus auprès d'ETW. Cela garantit que les événements ne seront pas supprimés par ETW.  
  
-   Gestion de l'envoi du flux d'événements à ETW. La cible ETW crée un événement ETW avec les données des Événements étendus et l'envoie à la session ETW appropriée. Si l'événement est plus grand que la taille de la mémoire tampon ou si les données ne peuvent pas s'ajuster dans un événement ETW, le suivi ETW fractionne l'événement en fragments.  
  
-   Conservation des packages des Événements étendus activés à tout instant.  
  
 Les emplacements de fichier par défaut suivants sont utilisés par ETW :  
  
-   Le fichier de sortie ETW se trouve dans %TEMP%\XEEtw.etl.  
  
    > [!IMPORTANT]  
    >  Le chemin d'accès ne peut pas être modifié une fois la première session démarrée.  
  
-   Les fichiers MOF (Managed Object Format) se trouvent dans le dossier *\<chemin de votre installation>* \Microsoft SQL Server\Shared. Pour plus d'informations, consultez [Format d'objet managé](http://go.microsoft.com/fwlink/?LinkId=92851) sur MSDN.  
  
## <a name="adding-the-target-to-a-session"></a>Ajout de la cible à une session  
 Pour ajouter la cible ETW à une session Événements étendus lorsque vous créez ou modifiez une session d'événements, vous devez inclure l'instruction suivante :  
  
```  
ADD TARGET package0.etw_classic_sync_target  
```  
  
 Pour plus d’informations et pour obtenir un exemple complet qui montre comment utiliser la cible ETW (et notamment comment afficher les données), consultez [Surveiller l’activité système à l’aide d’événements étendus](../../relational-databases/extended-events/monitor-system-activity-using-extended-events.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Cibles des Événements étendus SQL Server](http://msdn.microsoft.com/library/e281684c-40d1-4cf9-a0d4-7ea1ecffa384)   
 [sys.dm_xe_session_targets &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-xe-session-targets-transact-sql.md)   
 [CREATE EVENT SESSION &#40;Transact-SQL&#41;](../../t-sql/statements/create-event-session-transact-sql.md)   
 [ALTER EVENT SESSION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-event-session-transact-sql.md)  
  
  
