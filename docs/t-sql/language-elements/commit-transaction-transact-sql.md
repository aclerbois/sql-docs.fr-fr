---
title: COMMIT TRANSACTION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/09/2016
ms.prod: sql
ms.prod_service: sql-data-warehouse, database-engine, pdw, sql-database
ms.component: t-sql|language-elements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- COMMIT
- COMMIT TRANSACTION
- COMMIT_TSQL
- COMMIT_TRANSACTION_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- ending transactions [SQL Server]
- user-defined transactions [SQL Server]
- committed transactions
- transactions [SQL Server], ending
- marking end of transactions [SQL Server]
- implicit transactions
- distributed transactions [SQL Server], committed
- transactions [SQL Server], committed
- COMMIT TRANSACTION statement
- rolling back transactions, COMMIT TRANSACTION
ms.assetid: f8fe26a9-7911-497e-b348-4e69c7435dc1
caps.latest.revision: 53
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 95cf2d72d7e522d672ad791388a317cae8467ca3
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="commit-transaction-transact-sql"></a>COMMIT TRANSACTION (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-asdw-pdw-md](../../includes/tsql-appliesto-ss2008-asdb-asdw-pdw-md.md)]

  Marque la fin d'une transaction implicite ou explicite réussie. Si @@TRANCOUNT a la valeur 1, COMMIT TRANSACTION rend permanentes les modifications de données effectuées dans la base de données depuis le début de la transaction, libère les ressources détenues par la transaction et décrémente @@TRANCOUNT à 0. Si @@TRANCOUNT est supérieur à 1, COMMIT TRANSACTION décrémente @@TRANCOUNT de 1 seulement et la transaction reste active.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
-- Applies to SQL Server (starting with 2008) and Azure SQL Database
  
COMMIT [ { TRAN | TRANSACTION }  [ transaction_name | @tran_name_variable ] ] [ WITH ( DELAYED_DURABILITY = { OFF | ON } ) ]  
[ ; ]  
```  
 
```  
-- Applies to Azure SQL Data Warehouse and Parallel Data Warehouse Database
  
COMMIT [ TRAN | TRANSACTION ] 
[ ; ]  
``` 
 
  
## <a name="arguments"></a>Arguments  
 *transaction_name*  
 **S’APPLIQUE À :** SQL Server et Azure SQL Database
 
 Ignoré par le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]. *transaction_name* spécifie un nom de transaction attribué par une instruction BEGIN TRANSACTION antérieure. *transaction_name* doit être conforme aux règles applicables aux identificateurs, mais ne doit pas comporter plus de 32 caractères. *transaction_name* peut faciliter la lecture aux programmeurs en indiquant à quelle instruction BEGIN TRANSACTION imbriquée est associée l’instruction COMMIT TRANSACTION.  
  
 *@tran_name_variable*  
 **S’APPLIQUE À :** SQL Server et Azure SQL Database  
 
Nom d'une variable définie par l'utilisateur et contenant un nom de transaction valide. La variable doit être déclarée avec un type de données char, varchar, nchar ou nvarchar. Si plus de 32 caractères sont passés à la variable, seuls 32 caractères sont utilisés ; les autres caractères sont tronqués.  
  
 DELAYED_DURABILITY  
 **S’APPLIQUE À :** SQL Server et Azure SQL Database   

 Option qui demande que cette transaction soit validée avec durabilité différée. La demande est ignorée si la base de données a été modifiée avec `DELAYED_DURABILITY = DISABLED` ou `DELAYED_DURABILITY = FORCED`. Consultez la rubrique [Contrôler la durabilité d’une transaction](../../relational-databases/logs/control-transaction-durability.md) pour plus d’informations.  
  
## <a name="remarks"></a>Notes   
 Il est de la responsabilité des programmeurs [!INCLUDE[tsql](../../includes/tsql-md.md)] d'émettre une instruction COMMIT TRANSACTION seulement à un moment où toutes les données référencées par la transaction sont logiquement correctes.  
  
 Si la transaction validée est une transaction [!INCLUDE[tsql](../../includes/tsql-md.md)] distribuée, COMMIT TRANSACTION déclenche MS DTC pour utiliser un protocole de validation en deux phases qui valide tous les serveurs concernés par la transaction. Si une transaction locale affecte deux bases de données ou plus sur une même instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)], celle-ci utilise une validation interne en deux phases pour valider toutes les bases de données concernées par la transaction.  
  
 Dans les transactions imbriquées, la validation des transactions internes ne libère pas les ressources et ne rend pas leurs modifications permanentes. Les modifications de données sont rendues permanentes et les ressources ne sont libérées que lorsque la transaction externe est validée. Chaque instruction COMMIT TRANSACTION exécutée quand @@TRANCOUNT est supérieur à 1 décrémente simplement @@TRANCOUNT de 1. Quand @@TRANCOUNT atteint 0, la totalité de la transaction externe est validée. L’argument *transaction_name* étant ignoré par [!INCLUDE[ssDE](../../includes/ssde-md.md)], l’émission d’une instruction COMMIT TRANSACTION faisant référence au nom d’une transaction externe, tandis que des transactions internes sont en attente, réduit seulement @@TRANCOUNT de 1.  
  
 L’émission d’une instruction COMMIT TRANSACTION lorsque @@TRANCOUNT est égal à 0 génère un message d’erreur indiquant qu’il n’y a aucune instruction BEGIN TRANSACTION correspondante.  
  
 Il n'est plus possible d'annuler une transaction une fois l'instruction COMMIT TRANSACTION émise, car les modifications de données ont été enregistrées de manière permanente dans la base de données.  
  
 Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] incrémente le nombre de transactions pour une instruction d'une unité seulement lorsque ce nombre est égal à 0 au début de l'instruction.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle **public** .  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-committing-a-transaction"></a>A. Validation d’une transaction  
**S’APPLIQUE À :** SQL Server, Azure SQL Database, Azure SQL Data Warehouse et Parallel Data Warehouse   

L'exemple suivant supprime un candidat à un emploi. Il utilise AdventureWorks. 
  
```   
BEGIN TRANSACTION;   
DELETE FROM HumanResources.JobCandidate  
    WHERE JobCandidateID = 13;   
COMMIT TRANSACTION;   
```  
  
### <a name="b-committing-a-nested-transaction"></a>B. Validation d'une transaction imbriquée  
**S’APPLIQUE À :** SQL Server et Azure SQL Database    

L’exemple suivant crée une table, génère trois niveaux de transactions imbriquées, puis valide la transaction imbriquée. Bien que chaque instruction `COMMIT TRANSACTION` comporte un paramètre *transaction_name*, il n’existe aucune relation entre les instructions `COMMIT TRANSACTION` et `BEGIN TRANSACTION`. Les paramètres *transaction_name* aident simplement le programmeur à vérifier que le nombre correct de validations a été codé pour décrémenter `@@TRANCOUNT` jusqu’à 0, ce qui valide la transaction externe. 
  
```   
IF OBJECT_ID(N'TestTran',N'U') IS NOT NULL  
    DROP TABLE TestTran;  
GO  
CREATE TABLE TestTran (Cola int PRIMARY KEY, Colb char(3));  
GO  
-- This statement sets @@TRANCOUNT to 1.  
BEGIN TRANSACTION OuterTran;  
  
PRINT N'Transaction count after BEGIN OuterTran = '  
    + CAST(@@TRANCOUNT AS nvarchar(10));  
 
INSERT INTO TestTran VALUES (1, 'aaa');  
 
-- This statement sets @@TRANCOUNT to 2.  
BEGIN TRANSACTION Inner1;  
 
PRINT N'Transaction count after BEGIN Inner1 = '  
    + CAST(@@TRANCOUNT AS nvarchar(10));  
  
INSERT INTO TestTran VALUES (2, 'bbb');  
  
-- This statement sets @@TRANCOUNT to 3.  
BEGIN TRANSACTION Inner2;  
  
PRINT N'Transaction count after BEGIN Inner2 = '  
    + CAST(@@TRANCOUNT AS nvarchar(10));  
  
INSERT INTO TestTran VALUES (3, 'ccc');  
  
-- This statement decrements @@TRANCOUNT to 2.  
-- Nothing is committed.  
COMMIT TRANSACTION Inner2;  
 
PRINT N'Transaction count after COMMIT Inner2 = '  
    + CAST(@@TRANCOUNT AS nvarchar(10));  
 
-- This statement decrements @@TRANCOUNT to 1.  
-- Nothing is committed.  
COMMIT TRANSACTION Inner1;  
 
PRINT N'Transaction count after COMMIT Inner1 = '  
    + CAST(@@TRANCOUNT AS nvarchar(10));  
  
-- This statement decrements @@TRANCOUNT to 0 and  
-- commits outer transaction OuterTran.  
COMMIT TRANSACTION OuterTran;  
  
PRINT N'Transaction count after COMMIT OuterTran = '  
    + CAST(@@TRANCOUNT AS nvarchar(10));  
```  
  
## <a name="see-also"></a> Voir aussi  
 [BEGIN DISTRIBUTED TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/begin-distributed-transaction-transact-sql.md)   
 [BEGIN TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/begin-transaction-transact-sql.md)   
 [COMMIT WORK &#40;Transact-SQL&#41;](../../t-sql/language-elements/commit-work-transact-sql.md)   
 [ROLLBACK TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/rollback-transaction-transact-sql.md)   
 [ROLLBACK WORK &#40;Transact-SQL&#41;](../../t-sql/language-elements/rollback-work-transact-sql.md)   
 [SAVE TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/save-transaction-transact-sql.md)   
 [@@TRANCOUNT &#40;Transact-SQL&#41;](../../t-sql/functions/trancount-transact-sql.md)  
  
  
