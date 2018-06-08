---
title: SQL Server, objet SQL Statistics | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: performance-monitor
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- SQLServer:SQL Statistics
- SQL Statistics object
ms.assetid: da7dbb4b-f632-45a0-b1ab-c35cc2695c86
caps.latest.revision: 31
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 32a3d6216f05f8cb02d50a932a5c14f0fc91e442
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sql-server-sql-statistics-object"></a>Objet SQLServer:SQL Statistics
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  L’objet **SQLServer:SQL Statistics** dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournit des compteurs pour surveiller la compilation et le type de demandes transmises à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La surveillance du nombre de compilations et de recompilations de requêtes, ainsi que le nombre de lots reçus par une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vous donne une idée de la rapidité avec laquelle [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] traite les requêtes utilisateur et de l'efficacité avec laquelle l'optimiseur de requêtes traite les requêtes.  
  
 La compilation constitue une partie significative du délai d'exécution d'une requête. Pour économiser le coût de compilation, [!INCLUDE[ssDE](../../includes/ssde-md.md)] enregistre le plan de requête compilée dans un cache des requêtes. L'objectif du cache est de réduire la durée de compilation en conservant les requêtes compilées en vue d'une utilisation ultérieure, éliminant ainsi la nécessité de recompiler des requêtes si elles sont exécutées ultérieurement. Toutefois, chaque requête doit être compilée au moins une fois. La recompilation des requêtes peut être due aux facteurs suivants :  
  
-   modifications de schéma, y compris les modifications de schémas de base comme l'addition de colonnes ou d'index à une table, ou modifications de schémas statistiques comme l'insertion ou la suppression d'un nombre significatif de lignes dans une table ;  
  
-   modifications d'environnement (instruction SET). La modification des paramètres de session tels que ANSI_PADDING ou ANSI_NULLS peuvent provoquer la recompilation d'une requête.  
  
 Pour plus d’informations sur le paramétrage simple et forcé, consultez [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md).  
  
 Voici les compteurs **SQL Statistics** de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
|Compteurs statistiques SQL de SQL Server|Description|  
|----------------------------------------|-----------------|  
|**Nombre de tentatives d'autoparamétrage/s**|Nombre de tentatives d'autoparamétrage par seconde. Le total devrait être la somme des autoparamétrages qui sont sûrs, non sûrs ou qui ont échoué. L'autoparamétrage a lieu lorsqu'une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tente de paramétrer une demande [!INCLUDE[tsql](../../includes/tsql-md.md)] en remplaçant certains littéraux par des paramètres pour permettre la réutilisation du plan d'exécution résultant en cache sur plusieurs demandes d'apparence comparable. Notez que les autoparamétrages sont également appelés paramétrages simples dans les versions plus récentes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Ce compteur ne tient pas compte des paramétrages forcés.|  
|**Nombre de requêtes de lots/s**|Nombre de lots de commandes [!INCLUDE[tsql](../../includes/tsql-md.md)] reçus par seconde. Cette statistique est affectée par toutes les contraintes (comme les E/S, le nombre d'utilisateurs, la taille du cache, la complexité des requêtes, etc.). Un nombre élevé de requêtes de lots traduit un bon débit.|  
|**Nombre d'échecs d'autoparamétrage/s**|Nombre d'échecs de tentatives d'autoparamétrage par seconde. Ce nombre doit être faible. Notez que les autoparamétrages sont également appelés paramétrages simples dans les versions ultérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**Paramétrages forcés/s**|Nombre de paramétrages forcés réussis par seconde.|  
|**Exécutions guidées du plan/s**|Nombre d'exécutions de plan par seconde dans lesquelles le plan de requête a été généré à l'aide d'un repère de plan.|  
|**Exécutions du plan non correctement guidées/s**|Nombre d'exécutions de plan par seconde dans lesquelles un repère de plan n'a pas pu être honoré au cours de la génération du plan. Le repère de plan a été ignoré et la compilation normale a été utilisée pour générer le plan exécuté.|  
|**Nombre d'autoparamétrages sûrs/s**|Nombre de tentatives d'autoparamétrage sûres par seconde. Le qualificatif « sûr » fait référence à la détermination selon laquelle un plan d'exécution mis en cache peut être partagé entre plusieurs instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] d'apparence comparable. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] effectue de nombreuses tentatives d’autoparamétrage, dont certaines se révèlent sûres alors que d’autres échouent. Notez que les autoparamétrages sont également appelés paramétrages simples dans les versions ultérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Ce compteur ne tient pas compte des paramétrages forcés.|  
|**Taux d'avertissements SQL**|Nombre d'avertissements par seconde. Un avertissement est une demande émise par le client pour terminer la demande en cours d'exécution.|  
|**Compilations SQL/s**|Nombre de compilations SQL par seconde. Indique le nombre de fois où le chemin d'accès du code de compilation est entré. Inclut les compilations provoquées par les recompilations au niveau des instructions dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Une fois l'activité de l'utilisateur de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] stable, cette valeur atteint un état d'équilibre.|  
|**Recompilations SQL/s**|Nombre de recompilations d'instructions par seconde. Compte le nombre de fois où les recompilations d'instruction sont déclenchées. En général, il faut que le nombre de recompilations soit faible.|  
|**Nombre d'autoparamétrages non sûrs/s**|Nombre de tentatives d'autoparamétrage non sûres par seconde. Par exemple, la requête possède des caractéristiques qui empêchent le partage du plan mis en cache. Elles sont qualifiées de non sûres. Ce compteur ne tient pas compte du nombre de paramétrages forcés.|  
  
## <a name="see-also"></a> Voir aussi  
 [SQL Server - Objet Plan Cache](../../relational-databases/performance-monitor/sql-server-plan-cache-object.md)   
 [Analyser l’utilisation des ressources &#40;Moniteur système&#41;](../../relational-databases/performance-monitor/monitor-resource-usage-system-monitor.md)  
  
  
