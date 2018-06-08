---
title: Transactions (SQL Data Warehouse) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-data-warehouse, pdw
ms.component: t-sql|language-elements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: 87e5e593-a121-4428-9d3c-3af876224e35
caps.latest.revision: 8
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest || = sqlallproducts-allversions'
ms.openlocfilehash: 583e8146a43ea88066a5e0ac223ab2088c0c61ed
ms.sourcegitcommit: d2573a8dec2d4102ce8882ee232cdba080d39628
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/07/2018
---
# <a name="transactions-sql-data-warehouse"></a>Transactions (SQL Data Warehouse)
[!INCLUDE[tsql-appliesto-xxxxxx-xxxx-asdw-pdw-md](../../includes/tsql-appliesto-xxxxxx-xxxx-asdw-pdw-md.md)]

  Une transaction est un groupe d’une ou plusieurs instructions de base de données entièrement validées ou restaurées. Chaque transaction est atomique, cohérente, isolée et durable (ACID). Si la transaction réussit, toutes les instructions qu’elle contient sont validées. Si la transaction échoue, autrement dit si au moins une des instructions du groupe échoue, la totalité du groupe est restauré.  
  
 Le début et la fin des transactions varient selon le paramètre AUTOCOMMIT et les instructions BEGIN TRANSACTION, COMMIT et ROLLBACK. [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] prend en charge les types de transactions suivants :  
  
-   Les *transactions explicites* commencent avec l’instruction BEGIN TRANSACTION et se terminent avec l’instruction COMMIT ou ROLLBACK.  
  
-   Les *transactions de validation automatique* se lancent automatiquement au sein d’une session et ne commencent pas par l’instruction BEGIN TRANSACTION. Quand le paramètre AUTOCOMMIT a la valeur ON, chaque instruction s’exécute dans une transaction et aucune instruction COMMIT ou ROLLBACK explicite n’est nécessaire. Lorsque le paramètre AUTOCOMMIT a la valeur OFF, une instruction COMMIT ou ROLLBACK est nécessaire pour déterminer le résultat de la transaction. Dans [!INCLUDE[ssSDW](../../includes/sssdw-md.md)], les transactions validées automatiquement commencent immédiatement après une instruction COMMIT ou ROLLBACK, ou bien après une instruction SET AUTOCOMMIT OFF.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône Lien de rubrique") [Conventions de la syntaxe Transact-SQL &#40;Transact-SQL&#41;](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
BEGIN TRANSACTION [;]  
COMMIT [ TRAN | TRANSACTION | WORK ] [;]  
ROLLBACK [ TRAN | TRANSACTION | WORK ] [;]  
SET AUTOCOMMIT { ON | OFF } [;]  
SET IMPLICIT_TRANSACTIONS { ON | OFF } [;]  
```  
  
## <a name="arguments"></a>Arguments  
 BEGIN TRANSACTION  
 Indique le point de départ d’une transaction explicite.  
  
 COMMIT [ WORK ]  
 Marque la fin d’une transaction explicite ou de validation automatique. Cette instruction a pour effet de valider définitivement les modifications apportées à la transaction dans la base de données. L’instruction COMMIT est identique à COMMIT WORK, COMMIT TRAN et COMMIT TRANSACTION.  
  
 ROLLBACK [ WORK ]  
 Restaure une transaction au début de cette transaction. Aucune modification apportée à la transaction n’est validée dans la base de données. L’instruction ROLLBACK est identique à ROLLBACK WORK, ROLLBACK TRAN et ROLLBACK TRANSACTION.  
  
 SET AUTOCOMMIT { **ON** | OFF }  
 Détermine la façon dont les transactions peuvent commencer et se terminer.  
  
 ON  
 Chaque instruction s’exécute sous sa propre transaction et aucune instruction COMMIT ou ROLLBACK explicite n’est nécessaire. Les transactions explicites sont autorisées quand AUTOCOMMIT a la valeur ON.  
  
 OFF  
 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] lance automatiquement une transaction quand aucune transaction n’est déjà en cours. Toutes les instructions suivantes sont exécutées dans le cadre de la transaction et une instruction COMMIT ou ROLLBACK est nécessaire pour déterminer le résultat de la transaction. Dès qu’une transaction est validée ou restaurée sous ce mode de fonctionnement, le mode garde la valeur OFF et [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] lance une nouvelle transaction. Les transactions explicites ne sont pas autorisées quand AUTOCOMMIT a la valeur OFF.  
  
 Si le paramètre AUTOCOMMIT est modifié pendant une transaction active, il n’affecte pas cette transaction car il ne prend pas effet tant qu’elle n’est pas terminée.  
  
 Si AUTOCOMMIT a la valeur ON, l’exécution d’une autre instruction SET AUTOCOMMIT ON n’a aucun effet. De même, si AUTOCOMMIT a la valeur OFF, l’exécution d’une autre instruction SET AUTOCOMMIT OFF n’a aucun effet.  
  
 SET IMPLICIT_TRANSACTIONS { ON | **OFF** }  
 Cette instruction active ou désactive les mêmes modes que l’instruction SET AUTOCOMMIT. Si sa valeur est ON, SET IMPLICIT_TRANSACTIONS met la connexion en mode de transaction implicite. Si la valeur est OFF, la connexion est remise en mode de validation automatique.  Pour plus d’informations, consultez [SET IMPLICIT_TRANSACTIONS &#40;Transact-SQL&#41;](../../t-sql/statements/set-implicit-transactions-transact-sql.md).  
  
## <a name="permissions"></a>Autorisations  
 Aucune autorisation spécifique n’est nécessaire pour exécuter les instructions relatives aux transactions. En revanche, des autorisations sont nécessaires pour exécuter les instructions au sein de la transaction.  
  
## <a name="error-handling"></a>Gestion des erreurs  
 Si des instructions COMMIT ou ROLLBACK sont exécutées alors qu’il n’existe aucune transaction active, une erreur est générée.  
  
 Si une instruction BEGIN TRANSACTION est exécutée alors qu’une transaction est déjà en cours, une erreur est générée. Ce cas de figure peut se produire si une instruction BEGIN TRANSACTION se produit après une instruction BEGIN TRANSACTION réussie ou quand la session est en mode SET AUTOCOMMIT OFF.  
  
 Si une erreur autre qu’une erreur d’instruction au moment de l’exécution entrave le bon déroulement d’une transaction explicite, [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] la restaure automatiquement et libère toutes les ressources bloquées par la transaction. Par exemple, si la connexion réseau du client à une instance de [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] est interrompue ou que le client se déconnecte de l’application, toutes les transactions non validées pendant la connexion sont restaurées quand le réseau notifie l’instance de l’interruption.  
  
 Si une erreur d’instruction au moment de l’exécution se produit dans un lot, le comportement de [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] est conforme à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **XACT_ABORT** dont la valeur est **ON** et toute la transaction est restaurée. Pour plus d’informations sur le paramètre **XACT_ABORT**, consultez [SET XACT_ABORT (Transact-SQL)](http://msdn.microsoft.com/library/ms188792.aspx).  
  
## <a name="general-remarks"></a>Remarques d'ordre général  
 Une session ne peut exécuter qu’une seule transaction à un moment donné ; les points de sauvegarde et les transactions imbriquées ne sont pas pris en charge.  
  
 Il incombe aux programmeurs [!INCLUDE[DWsql](../../includes/dwsql-md.md)] d’émettre une instruction COMMIT seulement à un moment où toutes les données référencées par la transaction sont logiquement correctes.  
  
 Quand une session est arrêtée avant la fin d’une transaction, cette dernière est restaurée.  
  
 Les modes de transaction sont gérés session par session. Par exemple, si une session commence une transaction explicite ou affecte à AUTOCOMMIT la valeur OFF, ou bien affecte à IMPLICIT_TRANSACTIONS la valeur ON, cela n’a aucun effet sur les modes de transaction de toute autre session.  
  
## <a name="limitations-and-restrictions"></a>Limitations et restrictions  
 Il n’est plus possible de restaurer une transaction après l’émission d’une instruction COMMIT, car les modifications de données ont été définitivement enregistrées dans la base de données.  
  
 Les commandes [CREATE DATABASE &#40;Azure SQL Data Warehouse&#41;](../../t-sql/statements/create-database-azure-sql-data-warehouse.md) et [DROP DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-database-transact-sql.md) ne sont pas utilisables dans une transaction explicite.  
  
 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] n’a pas de mécanisme de partage de transaction. Cela implique qu’à tout moment, une seule session peut travailler sur une même transaction dans le système.  
  
## <a name="locking-behavior"></a>Comportement de verrouillage  
 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] utilise le verrouillage pour garantir l’intégrité des transactions et gérer la cohérence des bases de données quand plusieurs utilisateurs accèdent simultanément aux données. Le verrouillage est utilisé à la fois par les transactions implicites et explicites. Chaque transaction demande des verrous de différents types sur les ressources, comme les tables ou les bases de données dont elle dépend. Tous les verrous [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] se trouvent au niveau de la table ou au-dessus. Les verrous demandés empêchent les autres transactions d'apporter aux ressources des modifications susceptibles de nuire à la transaction. Chaque transaction libère ses verrous quand elle ne dépend plus des ressources verrouillées ; les transactions explicites conservent les verrous jusqu’à la fin de la transaction, une fois que celle-ci est validée ou restaurée.  
  
## <a name="examples-includesssdwfullincludessssdwfull-mdmd-and-includesspdwincludessspdw-mdmd"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="a-using-an-explicit-transaction"></a>A. Utilisation d’une transaction explicite  
  
```  
BEGIN TRANSACTION;  
DELETE FROM HumanResources.JobCandidate  
    WHERE JobCandidateID = 13;  
COMMIT;  
```  
  
### <a name="b-rolling-back-a-transaction"></a>B. Restauration d’une transaction  
 L’exemple suivant montre l’effet de la restauration d’une transaction.  Dans cet exemple, l’instruction ROLLBACK restaure l’instruction INSERT, mais la table créée continue d’exister.  
  
```  
CREATE TABLE ValueTable (id int);  
BEGIN TRANSACTION;  
       INSERT INTO ValueTable VALUES(1);  
       INSERT INTO ValueTable VALUES(2);  
ROLLBACK;  
```  
  
### <a name="c-setting-autocommit"></a>C. Définition du paramètre AUTOCOMMIT  
 L’exemple suivant affecte au paramètre AUTOCOMMIT la valeur `ON`.  
  
```  
SET AUTOCOMMIT ON;  
```  
  
 L’exemple suivant affecte au paramètre AUTOCOMMIT la valeur `OFF`.  
  
```  
SET AUTOCOMMIT OFF;  
```  
  
### <a name="d-using-an-implicit-multi-statement-transaction"></a>D. Utilisation d’une transaction implicite à plusieurs instructions  
  
```  
SET AUTOCOMMIT OFF;  
CREATE TABLE ValueTable (id int);  
INSERT INTO ValueTable VALUES(1);  
INSERT INTO ValueTable VALUES(2);  
COMMIT;  
```  
  
## <a name="see-also"></a> Voir aussi  
 [SET IMPLICIT_TRANSACTIONS &#40;Transact-SQL&#41;](../../t-sql/statements/set-implicit-transactions-transact-sql.md)   
 [SET TRANSACTION ISOLATION LEVEL &#40;Transact-SQL&#41;](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md)   
 [@@TRANCOUNT &#40;Transact-SQL&#41;](../../t-sql/functions/trancount-transact-sql.md)  
  
  
