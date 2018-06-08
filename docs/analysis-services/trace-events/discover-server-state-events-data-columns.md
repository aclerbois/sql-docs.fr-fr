---
title: Colonnes de données d’événements état du serveur de découverte | Documents Microsoft
ms.date: 05/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: trace-events
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: af00bfd3d38f7014efaa04037ffdd0754165e232
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="discover-server-state-events-data-columns"></a>Colonnes de données des événements de découverte d'état du serveur
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  La catégorie d'événement Découverte de l'état du serveur contient les classes d'événements suivantes :  
  
|**ID d'événement**|**Nom d'événement**|**Description de l'événement**|  
|------------------|--------------------|---------------------------|  
|33|Server State Discover Begin|Début de la découverte de l'état du serveur.|  
|34|Server State Discover Data|Contenu de la réponse de la découverte de l'état du serveur.|  
|35|Server State Discover End|Fin de la découverte de l'état du serveur.|  
  
 Les tableaux suivants répertorient les colonnes de données de chacune de ces classes d'événements.  
  
## <a name="server-state-discover-begin-classdata-columns"></a>Classe Server State Discover Begin—Colonnes de données  
  
|||||  
|-|-|-|-|  
|**Nom de la colonne**|**ID de la colonne**|**Type de colonne**|**Description de la colonne**|  
|EventClass|0|1|Classe d'événements utilisée pour catégoriser les événements.|  
|EventSubclass|1|1|La sous-classe d’événements fournit des informations supplémentaires sur chaque classe d’événements :<br /><br /> 1 : **DISCOVER_CONNECTIONS**<br /><br /> 2 : **DISCOVER_SESSIONS**<br /><br /> 3 : **DISCOVER_TRANSACTIONS**<br /><br /> 6 : **DISCOVER_DB_CONNECTIONS**<br /><br /> 7 : **DISCOVER_JOBS**<br /><br /> 8 : **DISCOVER_LOCKS**<br /><br /> 12 : **DISCOVER_PERFORMANCE_COUNTERS**<br /><br /> 13 : **DISCOVER_MEMORYUSAGE**<br /><br /> 14 : **DISCOVER_JOB_PROGRESS**<br /><br /> 15 : **DISCOVER_MEMORYGRANT**|  
|CurrentTime|2|5|Contient l'heure actuelle de l'événement de découverte de l'état du serveur, le cas échéant. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|StartTime|3|5|Contient l'heure de début de l'événement, le cas échéant. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|ConnectionID|25|1|Contient l'ID de connexion unique associé à l'événement de découverte de l'état du serveur.|  
|NTUserName|32|8|Contient le compte d'utilisateur Windows associé à l'événement de découverte de l'état du serveur.|  
|NTDomainName|33|8|Contient le compte de domaine Windows associé à l'événement de découverte de l'état du serveur.|  
|ClientProcessID|36|1|Contient l'ID de processus de l'application cliente qui a créé la connexion au serveur.|  
|ApplicationName|37|8|Contient le nom de l'application cliente qui a créé la connexion au serveur. Cette colonne est remplie avec les valeurs passées par l'application plutôt que par le nom affiché du programme.|  
|SessionID|39|8|Contient l'ID de session associé à l'événement de découverte de l'état du serveur.|  
|NTCanonicalUserName|40|8|Contient le nom d'utilisateur Windows associé à l'événement de découverte de l'état du serveur. Le nom d'utilisateur a la forme canonique. engineering.microsoft.com/software/user, par exemple.|  
|SPID|41|1|Contient l'ID de processus serveur (SPID) qui identifie de manière unique la session utilisateur associée à l'événement de découverte de l'état du serveur Le SPID correspond directement au GUID de session utilisé par XMLA.|  
|TextData|42|9|Contient les données texte associées à l'événement.|  
|ServerName|43|8|Contient le nom de l'instance [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] sur laquelle l'événement de découverte de l'état du serveur s'est produit.|  
|RequestProperties|45|9|Contient les propriétés de la requête XMLA active.|  
  
## <a name="server-state-discover-data-classdata-columns"></a>Classe Server State Discover Data—Colonnes de données  
  
|||||  
|-|-|-|-|  
|**Nom de la colonne**|**ID de la colonne**|**Type de colonne**|**Description de la colonne**|  
|EventClass|0|1|Classe d'événements utilisée pour catégoriser les événements.|  
|EventSubclass|1|1|La sous-classe d’événements fournit des informations supplémentaires sur chaque classe d’événements :<br /><br /> 1 : **DISCOVER_CONNECTIONS**<br /><br /> 2 : **DISCOVER_SESSIONS**<br /><br /> 3 : **DISCOVER_TRANSACTIONS**<br /><br /> 6 : **DISCOVER_DB_CONNECTIONS**<br /><br /> 7 : **DISCOVER_JOBS**<br /><br /> 8 : **DISCOVER_LOCKS**<br /><br /> 12 : **DISCOVER_PERFORMANCE_COUNTERS**<br /><br /> 13 : **DISCOVER_MEMORYUSAGE**<br /><br /> 14 : **DISCOVER_JOB_PROGRESS**<br /><br /> 15 : **DISCOVER_MEMORYGRANT**|  
|CurrentTime|2|5|Contient l'heure actuelle de l'événement de découverte de l'état du serveur, le cas échéant. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|StartTime|3|5|Contient l'heure de début de l'événement, le cas échéant. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|ConnectionID|25|1|Contient l'ID de connexion unique associé à l'événement de découverte de l'état du serveur.|  
|SessionID|39|8|Contient l'ID de session associé à l'événement de découverte de l'état du serveur.|  
|SPID|41|1|Contient l'ID de processus serveur (SPID) qui identifie de manière unique la session utilisateur associée à l'événement de découverte de l'état du serveur Le SPID correspond directement au GUID de session utilisé par XMLA.|  
|TextData|42|9|Contient les données texte associées à la réponse du serveur à la demande de découverte.|  
|ServerName|43|8|Contient le nom de l'instance [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] sur laquelle l'événement de découverte de l'état du serveur s'est produit.|  
  
## <a name="server-state-discover-end-classdata-columns"></a>Classe Server State Discover End—Colonnes de données  
  
|||||  
|-|-|-|-|  
|**Nom de la colonne**|**ID de la colonne**|**Type de colonne**|**Description de la colonne**|  
|EventClass|0|1|Classe d'événements utilisée pour catégoriser les événements.|  
|EventSubclass|1|1|La sous-classe d’événements fournit des informations supplémentaires sur chaque classe d’événements :<br /><br /> 1 : **DISCOVER_CONNECTIONS**<br /><br /> 2 : **DISCOVER_SESSIONS**<br /><br /> 3 : **DISCOVER_TRANSACTIONS**<br /><br /> 6 : **DISCOVER_DB_CONNECTIONS**<br /><br /> 7 : **DISCOVER_JOBS**<br /><br /> 8 : **DISCOVER_LOCKS**<br /><br /> 12 : **DISCOVER_PERFORMANCE_COUNTERS**<br /><br /> 13 : **DISCOVER_MEMORYUSAGE**<br /><br /> 14 : **DISCOVER_JOB_PROGRESS**<br /><br /> 15 : **DISCOVER_MEMORYGRANT**|  
|CurrentTime|2|5|Contient l'heure actuelle de l'événement de découverte de l'état du serveur, le cas échéant. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|StartTime|3|5|Contient l'heure de début de l'événement, le cas échéant. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|EndTime|4|5|Contient l'heure de fin de l'événement. Cette colonne n'est pas remplie pour les classes d'événements de démarrage, comme SQL:BatchStarting ou SP:Starting. Pour le filtrage, les formats attendus sont « YYYY-MM-DD » et « YYYY-MM-DD HH:MM:SS ».|  
|Duration|5|2|Contient le temps (en millisecondes) utilisé par l'événement.|  
|CPUTime|6|2|Contient le temps processeur (en millisecondes) utilisé par l'événement de découverte de l'état du serveur.|  
|ConnectionID|25|1|Contient l'ID de connexion unique associé à l'événement de découverte de l'état du serveur.|  
|NTUserName|32|8|Contient le compte d'utilisateur Windows associé à l'événement de découverte de l'état du serveur.|  
|NTDomainName|33|8|Contient le compte de domaine Windows associé à l'événement de découverte de l'état du serveur.|  
|ClientProcessID|36|1|Contient l'ID de processus de l'application cliente qui a initialisé la requête XMLA.|  
|ApplicationName|37|8|Contient le nom de l'application cliente qui a créé la connexion au serveur. Cette colonne est remplie avec les valeurs passées par l'application plutôt que par le nom affiché du programme.|  
|SessionID|39|8|Contient le compte de domaine Windows associé à l'événement de découverte de l'état du serveur.|  
|NTCanonicalUserName|40|8|Contient le nom d'utilisateur Windows associé à l'événement de découverte de l'état du serveur. Le nom d'utilisateur a la forme canonique. engineering.microsoft.com/software/user, par exemple.|  
|SPID|41|1|Contient l'ID de processus serveur (SPID) qui identifie de manière unique la session utilisateur associée à l'événement de découverte de l'état du serveur Le SPID correspond directement au GUID de session utilisé par XMLA.|  
|TextData|42|9|Contient les données texte associées à la réponse du serveur à la demande de découverte.|  
|ServerName|43|8|Contient le nom de l'instance [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] sur laquelle l'événement de découverte de l'état du serveur s'est produit.|  
  
## <a name="see-also"></a>Voir aussi  
 [Découverte, catégorie d’événement état du serveur](../../analysis-services/trace-events/discover-server-state-event-category.md)  
  
  
