---
title: DBCC FREESYSTEMCACHE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/16/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- FREESYSTEMCACHE_TSQL
- DBCC_FREESYSTEMCACHE_TSQL
- DBCC FREESYSTEMCACHE
- FREESYSTEMCACHE
dev_langs:
- TSQL
helpviewer_keywords:
- clearing unused cache entries
- DBCC FREESYSTEMCACHE statement
- unused cache entries
- releasing unused cache entries
- freeing unused cache entries
- cleaning unused cache entries
ms.assetid: 4b5c460b-e4ad-404a-b4ca-d65aba38ebbb
caps.latest.revision: 35
author: uc-msft
ms.author: umajay
manager: craigg
ms.openlocfilehash: 75c98aca474af0ebf02fd446cf9645fdb7c20373
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="dbcc-freesystemcache-transact-sql"></a>DBCC FREESYSTEMCACHE (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

Libère les entrées non utilisées de tous les caches. Le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] nettoie, en arrière-plan et de manière proactive, les entrées de cache non utilisées afin de libérer de la mémoire pour les entrées actives. Vous pouvez toutefois utiliser cette commande pour supprimer manuellement les entrées non utilisées de tous les caches ou d'un cache de pool du gouverneur de ressources spécifié.
  
![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntaxe  
```sql
DBCC FREESYSTEMCACHE   
    ( 'ALL' [, pool_name ] )   
    [WITH   
    { [ MARK_IN_USE_FOR_REMOVAL ] , [ NO_INFOMSGS ]  }  
    ]  
```  
  
## <a name="arguments"></a>Arguments  
 ( 'ALL' [,*pool_name* ] )  
 ALL spécifie tous les caches pris en charge.  
 *pool_name* spécifie un cache de pool de Resource Governor. Seules les entrées associées à ce pool seront libérées.  
  
 MARK_IN_USE_FOR_REMOVAL  
 Libère de manière asynchrone les entrées en cours d'utilisation de leurs caches respectifs une fois qu'elles ne sont plus utilisées. Ne sont pas affectées les entrées créées dans le cache après l'exécution de l'instruction DBCC FREESYSTEMCACHE WITH MARK_IN_USE_FOR_REMOVAL.  
  
 NO_INFOMSGS  
 Supprime tous les messages d'information.  
  
## <a name="remarks"></a>Notes   
L'exécution de DBCC FREESYSTEMCACHE efface le cache du plan pour l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cette opération entraîne la recompilation de tous les plans d'exécution ultérieurs et peut entraîner une baisse temporaire et brutale des performances des requêtes. Pour chaque mémoire cache effacée dans le cache de plan, le journal des erreurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] contient le message d'information suivant : « [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a rencontré %d occurrence(s) de vidages de mémoire cache pour la mémoire cache '%s' (partie du cache du plan) en raison d'opérations 'DBCC FREEPROCCACHE' ou 'DBCC FREESYSTEMCACHE' ». Ce message est enregistré toutes les cinq minutes si le cache est vidé au cours de cet intervalle de temps.

## <a name="result-sets"></a>Jeux de résultats  
DBCC FREESYSTEMCACHE retourne : « Exécution de DBCC terminée. Si DBCC vous a adressé des messages d'erreur, contactez l'administrateur système. »
  
## <a name="permissions"></a>Autorisations  
Nécessite l'autorisation ALTER SERVER STATE sur le serveur.
  
## <a name="examples"></a>Exemples  
  
### <a name="a-releasing-unused-cache-entries-from-a-resource-governor-pool-cache"></a>A. Libération des entrées de cache non utilisées d'un cache de pool du gouverneur de ressources  
L'exemple suivant illustre comment nettoyer des caches qui sont dédiés à un pool de ressources du gouverneur de ressources spécifié.
  
```sql
-- Clean all the caches with entries specific to the resource pool named "default".  
DBCC FREESYSTEMCACHE ('ALL', default);  
```  
  
### <a name="b-releasing-entries-from-their-respective-caches-after-they-become-unused"></a>B. Libération des entrées de leurs caches respectifs une fois qu'elles ne sont plus utilisées  
L'exemple suivant utilise la clause MARK_IN_USE_FOR_REMOVAL pour libérer les entrées de tous les caches actuels une fois qu'elles ne sont plus utilisées.
  
```sql
DBCC FREESYSTEMCACHE ('ALL') WITH MARK_IN_USE_FOR_REMOVAL;  
```  
  
## <a name="see-also"></a> Voir aussi  
[DBCC &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-transact-sql.md)  
[DBCC FREEPROCCACHE &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-freeproccache-transact-sql.md)  
[DBCC FREESESSIONCACHE &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-freesessioncache-transact-sql.md)  
[Resource Governor](../../relational-databases/resource-governor/resource-governor.md)
  
  
