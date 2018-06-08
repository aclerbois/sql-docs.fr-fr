---
title: DBCC FLUSHAUTHCACHE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- DBCC FLUSHAUTHCACHE
- FLUSHAUTHCACHE
- DBCC_FLUSHAUTHCACHE_TSQL
- FLUSHAUTHCACHE_TSQL
helpviewer_keywords:
- DBCC FLUSHAUTHCACHE
ms.assetid: 681ef31d-ceb9-4da5-86bf-bf1240df950f
caps.latest.revision: 11
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: = azuresqldb-current || = sqlallproducts-allversions
ms.openlocfilehash: 3cb7b06efe21d7dd6d4179c3454b6638ea636165
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="dbcc-flushauthcache-transact-sql"></a>DBCC FLUSHAUTHCACHE (Transact-SQL)
[!INCLUDE[tsql-appliesto-xxxxxx-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-xxxxxx-asdb-xxxx-xxx-md.md)]

Vide le cache d’authentification de la base de données contenant des informations sur les connexions et les règles de pare-feu, pour la base de données utilisateur actuelle dans [!INCLUDE[ssSDS](../../includes/sssds-md.md)]. Cette instruction ne s’applique pas à la base de données master logique, car la base de données master contient le stockage physique pour les informations sur les connexions et les règles de pare-feu. L’utilisateur qui exécute l’instruction et les autres utilisateurs actuellement connectés restent connectés. (DBCC FLUSHAUTHCACHE n’est pas pris en charge pour [!INCLUDE[ssSDW_md](../../includes/sssdw-md.md)].)
 
![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntaxe  
  
```sql
DBCC FLUSHAUTHCACHE [ ; ]  
```  
  
## <a name="arguments"></a>Arguments  
Aucun.
  
## <a name="remarks"></a>Notes   
Le cache d’authentification effectue une copie des connexions et des règles de pare-feu de serveur qui sont stockées dans la base de données master et les place en mémoire dans la base de données utilisateur.  Comme les informations sur les utilisateurs de la base de données à relation contenant-contenu sont déjà stockées dans la base de données utilisateur, ces utilisateurs ne font pas partie du cache d’authentification.
Des connexions actives en permanence à [!INCLUDE[ssSDS](../../includes/sssds-md.md)] nécessitent une réautorisation (effectuée par le [!INCLUDE[ssDE](../../includes/ssde-md.md)]) au moins toutes les 10 heures. Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] tente une réautorisation en utilisant le mot de passe envoyé à l’origine et aucune entrée utilisateur n’est nécessaire. Pour des raisons de performances, quand un mot de passe est réinitialisé dans [!INCLUDE[ssSDS](../../includes/sssds-md.md)], la connexion n’est pas authentifiée de nouveau, même si elle est réinitialisée suite à un regroupement de connexions. Cela est différent du comportement de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en local. Si le mot de passe a été modifié depuis que la connexion a été initialement autorisée, la connexion doit être interrompue et une nouvelle connexion établie à l’aide du nouveau mot de passe. Un utilisateur avec l’autorisation KILL DATABASE CONNECTION peut mettre fin explicitement à une connexion à [!INCLUDE[ssSDS](../../includes/sssds-md.md)] à l’aide de la commande [KILL &#40;Transact-SQL&#41;](../../t-sql/language-elements/kill-transact-sql.md).
  
## <a name="permissions"></a>Autorisations  
Nécessite le compte administrateur [!INCLUDE[ssSDS](../../includes/sssds-md.md)].
  
## <a name="example"></a> Exemple  
L’instruction suivante efface le cache d’authentification pour la base de données actuelle.
  
```sql
DBCC FLUSHAUTHCACHE;  
```  
  
## <a name="see-also"></a> Voir aussi  
[DBCC &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-transact-sql.md)
  
  
