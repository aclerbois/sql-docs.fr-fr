---
title: DBCC OPENTRAN (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- DBCC_OPENTRAN_TSQL
- DBCC OPENTRAN
- OPENTRAN_TSQL
- OPENTRAN
dev_langs:
- TSQL
helpviewer_keywords:
- status information [SQL Server], transactions
- transactions [SQL Server], status information
- DBCC OPENTRAN statement
- open transactions
- displaying transaction information
- checking open transactions
- oldest transactions [SQL Server]
ms.assetid: 63163843-226f-42d3-9e2c-b634fbf06943
caps.latest.revision: 40
author: uc-msft
ms.author: umajay
manager: craigg
ms.openlocfilehash: e12c70b7c455f6f3d39ba65f0543a8381bd9e6d2
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="dbcc-opentran-transact-sql"></a>DBCC OPENTRAN (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

L'instruction DBCC OPENTRAN aide à identifier les transactions actives qui peuvent empêcher la troncation du journal. Elle affiche les informations relatives à la transaction active la plus ancienne et aux transactions répliquées distribuées et non distribuées les plus anciennes, le cas échéant, dans le journal des transactions de la base de données spécifiée. Les résultats ne sont affichés que s'il existe une transaction active dans le journal ou si la base de données contient des informations de réplication. Un message d'information est affiché s'il n'y a pas de transactions actives dans le journal.
  
> [!NOTE]  
>  L'instruction DBCC OPENTRAN n'est pas prise en charge pour les serveurs de publication non-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntaxe  
  
```sql
DBCC OPENTRAN   
[   
    ( [ database_name | database_id | 0 ] ) ]  
    { [ WITH TABLERESULTS ]  
      [ , [ NO_INFOMSGS ] ]  
    }  
]   
```  
  
## <a name="arguments"></a>Arguments  
 *database_name* | *database_id*| 0  
 Nom ou ID de la base de données pour laquelle les informations de transaction les plus anciennes sont affichées. En l'absence de spécification, ou si 0 est spécifié, la base de données actuelle est utilisée. Les noms de base de données doivent suivre les règles applicables aux [identificateurs](../../relational-databases/databases/database-identifiers.md).  
  
 TABLERESULTS  
 Spécifie les résultats sous le format d'un tableau qui peut être chargé dans une table. Vous devez utiliser cette option pour créer une table de résultats pouvant être insérée dans une table à des fins de comparaison. Lorsque cette option n'est pas précisée, les résultats sont formatés pour pouvoir être lus.  
  
 NO_INFOMSGS  
 Supprime tous les messages d'information.  
  
## <a name="remarks"></a>Notes   
Vous devez utiliser DBCC OPENTRAN pour déterminer l'existence d'une transaction ouverte dans le journal des transactions. L'instruction BACKUP LOG ne permet de vider que la partie inactive du journal. Une transaction ouverte peut empêcher la troncature complète du journal. Pour identifier une transaction ouverte, utilisez sp_who pour obtenir l'ID du processus système.
  
## <a name="result-sets"></a>Jeux de résultats  
DBCC OPENTRAN renvoie le jeu de résultats suivant lorsqu'il n'y a pas de transactions ouvertes :
  
```sql
No active open transactions.  
DBCC execution completed. If DBCC printed error messages, contact your system administrator.  
```  
  
## <a name="permissions"></a>Autorisations  
Nécessite l’appartenance au rôle de serveur fixe **sysadmin** ou au rôle de base de données fixe **db_owner** .
  
## <a name="examples"></a>Exemples  
### <a name="a-returning-the-oldest-active-transaction"></a>A. Renvoi de la transaction active la plus ancienne  
L'exemple suivant obtient des informations de transaction pour la base de données active. Les résultats peuvent varier.
  
```sql  
CREATE TABLE T1(Col1 int, Col2 char(3));  
GO  
BEGIN TRAN  
INSERT INTO T1 VALUES (101, 'abc');  
GO  
DBCC OPENTRAN;  
ROLLBACK TRAN;  
GO  
DROP TABLE T1;  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
Transaction information for database 'master'.
Oldest active transaction:
SPID (server process ID) : 52
UID (user ID) : -1
Name          : user_transaction
LSN           : (518:1576:1)
Start time    : Jun  1 2004  3:30:07:197PM
SID           : 0x010500000000000515000000a065cf7e784b9b5fe77c87709e611500
DBCC execution completed. If DBCC printed error messages, contact your system administrator.
```
  
> [!NOTE]  
>  Le résultat « UID (ID d'utilisateur) » est sans signification et sera supprimé dans une future version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
### <a name="b-specifying-the-with-tableresults-option"></a>B. Utilisation de l'option WITH TABLERESULTS  
L'exemple suivant charge les résultats de la commande DBCC OPENTRAN dans une table temporaire.
  
```sql  
-- Create the temporary table to accept the results.  
CREATE TABLE #OpenTranStatus (  
   ActiveTransaction varchar(25),  
   Details sql_variant   
   );  
-- Execute the command, putting the results in the table.  
INSERT INTO #OpenTranStatus   
   EXEC ('DBCC OPENTRAN WITH TABLERESULTS, NO_INFOMSGS');  
  
-- Display the results.  
SELECT * FROM #OpenTranStatus;  
GO  
```  
  
## <a name="see-also"></a> Voir aussi  
[BEGIN TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/begin-transaction-transact-sql.md)  
[COMMIT TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/commit-transaction-transact-sql.md)  
[DBCC &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-transact-sql.md)  
[DB_ID &#40;Transact-SQL&#41;](../../t-sql/functions/db-id-transact-sql.md)  
[ROLLBACK TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/rollback-transaction-transact-sql.md)
  
  
