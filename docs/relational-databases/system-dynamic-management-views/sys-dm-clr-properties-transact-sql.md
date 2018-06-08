---
title: Sys.dm_clr_properties (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, pdw
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sys.dm_clr_properties
- sys.dm_clr_properties_TSQL
- dm_clr_properties_TSQL
- dm_clr_properties
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_clr_properties dynamic management view
ms.assetid: 220d062f-d117-46e7-a448-06fe48db8163
caps.latest.revision: 26
author: stevestein
ms.author: sstein
manager: craigg
monikerRange: '>= aps-pdw-2016 || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: f95f4d1596d84648034b51833738a26817f5e96b
ms.sourcegitcommit: 7019ac41524bdf783ea2c129c17b54581951b515
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/23/2018
---
# <a name="sysdmclrproperties-transact-sql"></a>sys.dm_clr_properties (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-pdw-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-pdw-md.md)]

  Renvoie une ligne pour chaque propriété relative à l'intégration du CLR (Common Language Runtime) [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], comprenant la version et l'état du CLR hébergé. Le CLR hébergé est initialisé en exécutant le [CREATE ASSEMBLY](../../t-sql/statements/create-assembly-transact-sql.md), [ALTER ASSEMBLY](../../t-sql/statements/alter-assembly-transact-sql.md), ou [DROP ASSEMBLY](../../t-sql/statements/drop-assembly-transact-sql.md) instructions, ou en exécutant une routine, un type ou un déclencheur CLR. Le **sys.dm_clr_properties** vue ne spécifie pas si l’exécution de code CLR utilisateur a été activée sur le serveur. L’exécution de code CLR utilisateur est activée à l’aide de la [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) procédure stockée avec la [clr activé](../../database-engine/configure-windows/clr-enabled-server-configuration-option.md) option définie sur 1.  
  
 Le **sys.dm_clr_properties** vue contient les **nom** et **valeur** colonnes. Chaque ligne de cette vue fournit des détails sur une propriété du CLR hébergé. Utilisez cette vue pour recueillir des informations sur le CLR hébergé, telles que le répertoire d'installation du CLR, sa version ou encore son état actuel. Cette vue peut vous aider à déterminer que le code d'intégration CLR ne fonctionne pas en raison de problèmes d'installation du CLR sur le serveur.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**nom**|**nvarchar(128)**|Nom de la propriété.|  
|**valeur**|**nvarchar(128)**|Valeur de la propriété.|  
  
## <a name="properties"></a>Propriétés  
 Le **répertoire** propriété indique le répertoire .NET Framework a été installé sur le serveur. Il existe plusieurs façons d'installer le .NET Framework sur le serveur et la valeur de cette propriété identifie l'installation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui est utilisée.  
  
 Le **version** propriété indique la version du .NET Framework et le CLR hébergé sur le serveur.  
  
 Le **sys.dm_clr_properties** vue de gestion dynamique peut renvoyer six valeurs différentes pour le **état** propriété, qui reflète l’état de la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] CLR hébergé. Celles-ci sont les suivantes :  
  
-   Mscoree is not loaded.  
  
-   Mscoree is loaded.  
  
-   Locked CLR version with mscoree.  
  
-   CLR is initialized.  
  
-   CLR initialization permanently failed.  
  
-   CLR is stopped.  
  
 Le **Mscoree n’est pas chargé** et **Mscoree est chargé** États afficher la progression de l’initialisation du CLR hébergé au démarrage du serveur et ne sont pas susceptibles d’être consultés.  
  
 Le **version CLR de verrouillage avec mscoree** état peut se produire lorsque le CLR hébergé n’est pas utilisé et, par conséquent, il n'a pas encore été initialisé. Le CLR hébergé est initialisé la première fois une instruction DDL (tel que [CREATE ASSEMBLY &#40;Transact-SQL&#41;](../../t-sql/statements/create-assembly-transact-sql.md)) ou un objet de base de données managés est exécuté.  
  
 Le **CLR est initialisé** état indique que le CLR hébergé a été correctement initialisé. Cela n'indique pas si l'exécution de code CLR utilisateur a été activée. Si l’exécution de code CLR utilisateur est d’abord activé et désactivé à l’aide de la [!INCLUDE[tsql](../../includes/tsql-md.md)] [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) une procédure stockée, la valeur d’état sera **CLR est initialisé**.  
  
 Le **CLR Échec d’initialisation définitivement** état indique que CLR hébergé échouée de l’initialisation. Cela est probablement dû à une insuffisance de mémoire ou à l'échec de la négociation d'hébergement entre [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et le CLR. Si tel est le cas, le message d'erreur 6512 ou 6513 est déclenché.  
  
 Le **CLR est arrêté état** est visible uniquement lorsque [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est en cours d’arrêt.  
  
## <a name="remarks"></a>Notes  
 Les propriétés et les valeurs de cette vue peuvent changer dans une future version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en raison des améliorations de la fonctionnalité d’intégration de CLR.  
  
## <a name="permissions"></a>Autorisations  
  
Sur [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)], nécessite `VIEW SERVER STATE` autorisation.   
Sur [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)], nécessite le `VIEW DATABASE STATE` autorisation dans la base de données.   

## <a name="examples"></a>Exemples  
 L'exemple suivant récupère des informations sur le CLR hébergé :  
  
```  
SELECT name, value   
FROM sys.dm_clr_properties;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions et vues de gestion dynamique &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Vues de gestion dynamique liées à Common Language Runtime &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/common-language-runtime-related-dynamic-management-views-transact-sql.md)  
  
  
