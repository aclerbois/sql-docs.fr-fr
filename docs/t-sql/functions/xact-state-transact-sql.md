---
title: XACT_STATE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: t-sql|functions
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- XACT_STATE
- XACT_STATE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- states [SQL Server], transactions
- uncommittable transactions
- sessions [SQL Server], active transactions
- XACT_STATE function
- transactions [SQL Server], state
- active transactions
ms.assetid: e9300827-e793-4eb6-9042-ffa0204aeb50
caps.latest.revision: 41
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 6b3293dab60c9b3f7f39b47c15397d29c5892ec9
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="xactstate-transact-sql"></a>XACT_STATE (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  Fonction scalaire qui indique l'état de la transaction utilisateur d'une demande en cours d'exécution. XACT_STATE indique s'il existe une transaction utilisateur active pour la demande en cours et si la transaction peut être validée.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
XACT_STATE()  
```  
  
## <a name="return-type"></a>Type de retour  
 **smallint**  
  
## <a name="remarks"></a>Notes   
 La fonction XACT_STATE renvoie les valeurs suivantes :  
  
|Valeur retournée|Signification|  
|------------------|-------------|  
| 1|Il existe une transaction utilisateur active pour la demande en cours. La demande peut exécuter toutes les actions, y compris l'écriture de données et la validation de la transaction.|  
|0|Il n'existe aucune transaction utilisateur active pour la demande en cours.|  
|-1|Il existe une transaction utilisateur active pour la demande en cours, mais une erreur a entraîné le classement de la transaction comme non validable. La demande ne peut pas valider la transaction ou revenir à un point d'enregistrement ; elle peut seulement demander une annulation complète de la transaction. La demande ne peut effectuer aucune opération d'écriture tant qu'elle n'a pas annulé la transaction. La demande peut effectuer uniquement des opérations de lecture tant qu'elle n'a pas annulé la transaction. Lorsque la transaction est annulée, la demande peut effectuer des opérations de lecture et d'écriture et commencer une nouvelle transaction.<br /><br /> À la fin de l’exécution du lot extérieur, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] restaure automatiquement toutes les transactions non validables actives. Si aucun message d'erreur n'a été envoyé lorsque la transaction est passée dans un état non validable, une erreur est envoyée à l'application cliente lorsque le traitement se termine. Ce message indique qu'une transaction non validable a été détectée et restaurée.|  
  
 Les fonctions XACT_STATE et @@TRANCOUNT permettent de détecter s’il existe une transaction utilisateur active pour la demande en cours. @@TRANCOUNT ne permet pas de déterminer si cette transaction a été classée comme transaction non validable. XACT_STATE ne permet pas de déterminer s'il existe des transactions imbriquées.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant utilise `XACT_STATE` dans le bloc `CATCH` d'une construction `TRY…CATCH` pour déterminer si une transaction doit être validée ou annulée. L'option `SET XACT_ABORT` étant active (`ON`), l'erreur de violation de contrainte place la transaction dans un état non validable.  
  
```  
USE AdventureWorks2012;  
GO  
  
-- SET XACT_ABORT ON will render the transaction uncommittable  
-- when the constraint violation occurs.  
SET XACT_ABORT ON;  
  
BEGIN TRY  
    BEGIN TRANSACTION;  
        -- A FOREIGN KEY constraint exists on this table. This   
        -- statement will generate a constraint violation error.  
        DELETE FROM Production.Product  
            WHERE ProductID = 980;  
  
    -- If the delete operation succeeds, commit the transaction. The CATCH  
    -- block will not execute.  
    COMMIT TRANSACTION;  
END TRY  
BEGIN CATCH  
    -- Test XACT_STATE for 0, 1, or -1.  
    -- If 1, the transaction is committable.  
    -- If -1, the transaction is uncommittable and should   
    --     be rolled back.  
    -- XACT_STATE = 0 means there is no transaction and  
    --     a commit or rollback operation would generate an error.  
  
    -- Test whether the transaction is uncommittable.  
    IF (XACT_STATE()) = -1  
    BEGIN  
        PRINT 'The transaction is in an uncommittable state.' +  
              ' Rolling back transaction.'  
        ROLLBACK TRANSACTION;  
    END;  
  
    -- Test whether the transaction is active and valid.  
    IF (XACT_STATE()) = 1  
    BEGIN  
        PRINT 'The transaction is committable.' +   
              ' Committing transaction.'  
        COMMIT TRANSACTION;     
    END;  
END CATCH;  
GO  
```  
  
## <a name="see-also"></a> Voir aussi  
 [@@TRANCOUNT &#40;Transact-SQL&#41;](../../t-sql/functions/trancount-transact-sql.md)   
 [BEGIN TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/begin-transaction-transact-sql.md)   
 [COMMIT TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/commit-transaction-transact-sql.md)   
 [ROLLBACK TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/rollback-transaction-transact-sql.md)   
 [SAVE TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/save-transaction-transact-sql.md)   
 [TRY...CATCH &#40;Transact-SQL&#41;](../../t-sql/language-elements/try-catch-transact-sql.md)  
  
  
