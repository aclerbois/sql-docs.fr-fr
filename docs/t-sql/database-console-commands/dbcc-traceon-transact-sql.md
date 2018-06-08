---
title: DBCC TRACEON (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/17/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- DBCC_TRACEON_TSQL
- TRACEON
- DBCC TRACEON
- TRACEON_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- DBCC TRACEON statement
- trace flags [SQL Server], enabling
ms.assetid: 93085324-ebaa-4e38-aac8-5e57b4b0d36d
caps.latest.revision: 36
author: uc-msft
ms.author: umajay
manager: craigg
ms.openlocfilehash: 2f9ae6abb4862a9b74534d24fa407e5a85f3eac6
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="dbcc-traceon-transact-sql"></a>DBCC TRACEON (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

Active les indicateurs de trace spécifiés.
  
![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntaxe  
  
```sql
DBCC TRACEON ( trace# [ ,...n ][ , -1 ] ) [ WITH NO_INFOMSGS ]  
```  
  
## <a name="arguments"></a>Arguments  
*trace#*  
Numéro de l'indicateur de trace à activer.  
  
*n*  
Espace réservé précisant qu'il est possible de spécifier plusieurs indicateurs de trace.  
  
-1  
Active globalement les indicateurs de trace spécifiés.  
  
WITH NO_INFOMSGS  
Supprime tous les messages d'information.  
  
## <a name="remarks"></a>Notes   
Sur un serveur de production, pour éviter un comportement imprévisible, il est recommandé d'activer uniquement les indicateurs de trace à l'échelle du serveur à l'aide de l'une des méthodes suivantes :
-   Utilisez l’option de démarrage de ligne de commande **-T** de Sqlservr.exe. Cette pratique est recommandée car elle garantit que toutes les instructions sont exécutées avec l'indicateur de trace activé. Celles-ci comprennent les commandes des scripts de démarrage. Pour plus d’informations, consultez [sqlservr Application](../../tools/sqlservr-application.md).  
-   Utilisez DBCC TRACEON **(***trace#* [**,** ...*.n*]**,-1)** uniquement quand des utilisateurs ou des applications ne sont pas simultanément en train d’exécuter des instructions sur le système.  

Les indicateurs de trace permettent de personnaliser certaines caractéristiques en contrôlant le fonctionnement de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Les indicateurs de trace, une fois activés, le restent sur le serveur jusqu'à ce qu'ils soient désactivés lors de l'exécution de l'instruction DBCC TRACEOFF. Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], il existe deux types d'indicateurs de trace : les indicateurs de trace de session et les indicateurs de trace globaux. Les indicateurs de trace de session sont actifs pour une connexion et visibles uniquement pour celle-ci. Les indicateurs de trace globaux sont définis au niveau du serveur et sont visibles pour chaque connexion sur celui-ci. Pour déterminer l'état des indicateurs de trace, utilisez l'instruction DBCC TRACESTATUS. Pour désactiver certains indicateurs de trace, exécutez DBCC TRACEOFF.
  
Après avoir activé un indicateur de trace qui affecte les plans de requête, exécutez `DBCC FREEPROCCACHE;` afin que les plans mis en cache soient recompilés à l’aide du nouveau comportement affectant le plan.
  
## <a name="result-sets"></a>Jeux de résultats  
 DBCC TRACEON retourne le jeu de résultats suivant (message) :  
  
```sql
DBCC execution completed. If DBCC printed error messages, contact your system administrator.  
```  
  
## <a name="permissions"></a>Autorisations  
Nécessite l'appartenance au rôle serveur fixe **sysadmin** .
  
## <a name="examples"></a>Exemples  
L'exemple suivant désactive la compression matérielle des lecteurs de bandes en activant l'indicateur de trace `3205`. Cet indicateur est uniquement activé pour la connexion active.
  
```sql  
DBCC TRACEON (3205);  
GO  
```  
  
L'exemple suivant active globalement l'indicateur de trace `3205`.
  
```sql  
DBCC TRACEON (3205, -1);  
GO  
```  
  
L'exemple suivant active globalement les indicateurs de trace `3205` et `260`.
  
```sql  
DBCC TRACEON (3205, 260, -1);  
GO  
```  
  
## <a name="see-also"></a> Voir aussi  
[DBCC &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-transact-sql.md)  
[DBCC TRACEOFF &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-traceoff-transact-sql.md)  
[DBCC TRACESTATUS &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-tracestatus-transact-sql.md)  
[Indicateurs de trace &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md)  
[Activer un comportement de l’optimiseur de requête SQL Server affectant le plan qui peut être contrôlé par différents indicateurs de trace à un niveau de requête spécifique](https://support.microsoft.com/kb/2801413)
  
  
