---
title: DBCC INPUTBUFFER (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2018
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- DBCC INPUTBUFFER
- INPUTBUFFER
- DBCC_INPUTBUFFER_TSQL
- INPUTBUFFER_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- input buffers [SQL Server]
- last statement from client
- displaying last statement sent
- statements [SQL Server], last statement
- DBCC INPUTBUFFER statement
ms.assetid: a44d702b-b3fb-4950-8c8f-1adcf3f514ba
caps.latest.revision: 51
author: uc-msft
ms.author: umajay
manager: craigg
ms.openlocfilehash: da4393aecd4371afc5a9a3cba0725b7bfa55b34c
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="dbcc-inputbuffer-transact-sql"></a>DBCC INPUTBUFFER (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

Affiche la dernière instruction envoyée depuis un client à une instance de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
  
![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntaxe  
  
```sql
DBCC INPUTBUFFER ( session_id [ , request_id ])  
[WITH NO_INFOMSGS ]  
```  
  
## <a name="arguments"></a>Arguments  
*session_id*  
ID de la session associé à chaque connexion principale active.  
  
*request_id*  
Demande exacte (traitement) de recherche dans la session active.  

La requête suivante retourne *request_id* :  
```sql
SELECT request_id   
FROM sys.dm_exec_requests   
WHERE session_id = @@spid;  
```  
par  
Permet de spécifier des options.  
  
NO_INFOMSGS  
Supprime tous les messages d'information dont les niveaux de gravité sont compris entre 0 et 10.  
  
## <a name="result-sets"></a>Jeux de résultats  
DBCC INPUTBUFFER retourne un jeu de lignes comportant les colonnes suivantes.
  
|Nom de colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**EventType**|**nvarchar(30)**|Type d'événement. Il peut s’agir d’un **événement RPC** ou d’un **événement de langage**. La sortie correspond à **Aucun événement** quand aucun dernier événement n’a été détecté.|  
|**Paramètres**|**smallint**|0 = texte<br /><br /> 1- *n* = Paramètres|  
|**EventInfo**|**nvarchar(4000)**|Pour un **EventType** de type RPC, **EventInfo** contient uniquement le nom de la procédure. Pour un **EventType** de type langage, seuls les 4 000 premiers caractères de l’événement sont affichés.|  
  
Par exemple, DBCC INPUTBUFFER retourne le jeu de résultats suivant lorsque le dernier événement dans le tampon est DBCC INPUTBUFFER(11) :
  
```
EventType      Parameters EventInfo               
-------------- ---------- ---------------------   
Language Event 0          DBCC INPUTBUFFER (11)  
  
(1 row(s) affected)  
  
DBCC execution completed. If DBCC printed error messages, contact your system administrator.  
```  

> [!NOTE]
> À compter de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2, utilisez [sys.dm_exec_input_buffer](../../relational-databases/system-dynamic-management-views/sys-dm-exec-input-buffer-transact-sql.md) pour retourner des informations sur les instructions envoyées à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

## <a name="permissions"></a>Autorisations  
Sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], l’une des conditions suivantes est requise :
-   L’utilisateur doit être membre du rôle serveur fixe **sysadmin**.  
-   L'utilisateur doit bénéficier de l'autorisation VIEW SERVER STATE.  
-   L’argument *session_id* doit être identique à l’ID de session dans laquelle la commande est exécutée. Pour déterminer l'ID de session, exécutez la requête suivante :  
  
```sql
SELECT @@spid;  
```
  
Pour les niveaux [!INCLUDE[ssSDS](../../includes/sssds-md.md)] Premium et critique pour l’entreprise, l’autorisation VIEW DATABASE STATE est requise dans la base de données. Pour les niveaux [!INCLUDE[ssSDS](../../includes/sssds-md.md)] Standard, De base et Usage général, le compte administrateur [!INCLUDE[ssSDS](../../includes/sssds-md.md)] est requis.
  
## <a name="examples"></a>Exemples  
L'exemple suivant exécute `DBCC INPUTBUFFER` sur une seconde connexion tandis qu'une longue transaction est en cours d'exécution sur une connexion précédente.
  
```sql
CREATE TABLE dbo.T1 (Col1 int, Col2 char(3));  
GO  
DECLARE @i int = 0;  
BEGIN TRAN  
SET @i = 0;  
WHILE (@i < 100000)  
BEGIN  
INSERT INTO dbo.T1 VALUES (@i, CAST(@i AS char(3)));  
SET @i += 1;  
END;  
COMMIT TRAN;  
--Start new connection #2.  
DBCC INPUTBUFFER (52);  
```  

## <a name="see-also"></a> Voir aussi  
[DBCC &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-transact-sql.md)  
[sp_who &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-who-transact-sql.md)  
[sys.dm_exec_input_buffer &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-input-buffer-transact-sql.md)
  
  
