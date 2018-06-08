---
title: sp_monitor (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_monitor_TSQL
- sp_monitor
dev_langs:
- TSQL
helpviewer_keywords:
- sp_monitor
ms.assetid: cb628496-2f9b-40e4-b018-d0831c4cb018
caps.latest.revision: 18
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 277062160e01f0111eeade2dc4a05b3c6a3ab59d
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spmonitor-transact-sql"></a>sp_monitor (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Affiche des statistiques sur [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_monitor  
```  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de colonne| Description|  
|-----------------|-----------------|  
|**last_run**|Temps **sp_monitor** de la dernière exécution.|  
|**current_run**|Temps **sp_monitor** est en cours d’exécution.|  
|**secondes**|Nombre de secondes écoulées depuis **sp_monitor** a été exécuté.|  
|**cpu_busy**|Nombre de secondes que l'UC de l'ordinateur serveur a consacrées à des tâches [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**io_busy**|Nombre de secondes que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a consacrées aux opérations d'entrée et de sortie.|  
|**Des temps d’inactivité**|Nombre de secondes pendant lesquelles [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a été inactif.|  
|**packets_received**|Nombre de paquets entrants lus par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**packets_sent**|Nombre de paquets sortants écrits par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**packet_errors**|Nombre d'erreurs détectées par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] lors de la lecture ou de l'écriture des paquets.|  
|**total_read**|Nombre de lectures effectuées par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**total_write**|Nombre d'écritures effectuées par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**total_errors**|Nombre d'erreurs détectées par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] lors des opérations de lecture et d'écriture.|  
|**Connexions**|Nombre de connexions ou de tentatives de connexion à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
  
## <a name="remarks"></a>Notes  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] assure le suivi, par le biais d'une série de fonctions, du volume de son activité. L’exécution de **sp_monitor** affiche les valeurs retournées par ces fonctions et montre combien ils ont été modifiés depuis la dernière exécution de la procédure.  
  
 Pour chaque colonne, les statistiques sont imprimées sous la forme *nombre*(*nombre*)-*nombre*% ou *nombre*(*nombre*). La première *nombre* fait référence au nombre de secondes (pour **cpu_busy**, **io_busy**, et **inactif**) ou le nombre total (pour les autres variables) depuis [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a été redémarré. Le *nombre* entre parenthèses correspond au nombre de secondes ou nombre total depuis la dernière **sp_monitor** a été exécuté. Le pourcentage est le pourcentage de temps depuis **sp_monitor** de la dernière exécution. Par exemple, si le rapport affiche **cpu_busy** comme 4250 (215)-68 %, le processeur est occupés secondes 4250 depuis [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dernier démarrage, pendant 215 secondes depuis **sp_monitor** était la dernière exécution et 68 pour cent de la durée totale depuis **sp_monitor** de la dernière exécution.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle serveur fixe **sysadmin** .  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant fournit des informations sur le niveau d'activité de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
```  
USE master  
EXEC sp_monitor  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
||||  
|-|-|-|  
|**last_run**|**current_run**|**secondes**|  
|29 mars 1998 11:55|4 avril 1998 14:22|561|  
  
||||  
|-|-|-|  
|**cpu_busy**|**io_busy**|**Des temps d’inactivité**|  
|190(0)-0%|187(0)-0%|148(556)-99%|  
  
||||  
|-|-|-|  
|**packets_received**|**packets_sent**|**packet_errors**|  
|16(1)|20(2)|0(0)|  
  
|||||  
|-|-|-|-|  
|**total_read**|**total_write**|**total_errors**|**Connexions**|  
|141(0)|54920(127)|0(0)|4(0)|  
  
## <a name="see-also"></a>Voir aussi  
 [sp_who &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-who-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
