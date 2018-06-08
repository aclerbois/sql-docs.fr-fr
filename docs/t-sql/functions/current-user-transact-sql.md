---
title: CURRENT_USER (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: t-sql|functions
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- CURRENT_USER
- CURRENT_USER_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- usernames [SQL Server]
- current user names
- niladic functions
- CURRENT_USER
- users [SQL Server], names
ms.assetid: 29248949-325b-4063-9f55-5a445fb35c6e
caps.latest.revision: 43
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 29765164d6eb5e677c307091cf37aa8623674f00
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="currentuser-transact-sql"></a>CURRENT_USER (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

Cette fonction retourne le nom de l’utilisateur actif. Cette fonction est équivalente à `USER_NAME()`.
  
![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntaxe  
  
```sql
CURRENT_USER  
```  

## <a name="return-types"></a>Types de retour
**sysname**
  
## <a name="remarks"></a>Notes   
`CURRENT_USER` retourne le nom du contexte de sécurité actuel. Si `CURRENT_USER` s’exécute après qu’un appel à `EXECUTE AS` change de contexte, `CURRENT_USER` retourne le nom du contexte qui a fait l’objet d’un emprunt d’identité. Si un principal Windows a accédé à la base de données du fait de son appartenance à un groupe, `CURRENT_USER` retourne le nom du principal Windows à la place du nom du groupe.
  
Pour savoir comment retourner le compte de connexion de l’utilisateur actif, consultez [SUSER_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/suser-name-transact-sql.md) et [SYSTEM_USER &#40;Transact-SQL&#41;](../../t-sql/functions/system-user-transact-sql.md).
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-currentuser-to-return-the-current-user-name"></a>A. Utilisation de CURRENT_USER pour obtenir le nom de l'utilisateur actuel  
Cet exemple retourne le nom de l’utilisateur actif.
  
```sql
SELECT CURRENT_USER;  
GO  
```  
  
### <a name="b-using-currentuser-as-a-default-constraint"></a>B. Utilisation de CURRENT_USER en tant que contrainte DEFAULT  
Cet exemple crée une table qui utilise `CURRENT_USER` comme contrainte `DEFAULT` pour la colonne `order_person` dans une ligne de ventes.
  
```sql
USE AdventureWorks2012;  
GO  
IF EXISTS (SELECT TABLE_NAME FROM INFORMATION_SCHEMA.TABLES  
      WHERE TABLE_NAME = 'orders22')  
   DROP TABLE orders22;  
GO  
SET NOCOUNT ON;  
CREATE TABLE orders22  
(  
order_id int IDENTITY(1000, 1) NOT NULL,
cust_id  int NOT NULL,
order_date smalldatetime NOT NULL DEFAULT GETDATE(),
order_amt money NOT NULL,
order_person char(30) NOT NULL DEFAULT CURRENT_USER
);  
GO  
```  
  
Cet exemple insère un enregistrement dans la table. L’utilisateur nommé `Wanida` exécute ces instructions.
  
```sql
INSERT orders22 (cust_id, order_amt)  
VALUES (5105, 577.95);  
GO  
SET NOCOUNT OFF;  
GO  
```  
  
Cette requête sélectionne toutes les informations dans la table `orders22`.
  
```sql
SELECT * FROM orders22;  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
order_id    cust_id     order_date           order_amt    order_person
----------- ----------- -------------------- ------------ ------------
1000        5105        2005-04-03 23:34:00  577.95       Wanida
  
(1 row(s) affected)
```
  
### <a name="c-using-currentuser-from-an-impersonated-context"></a>C. Utilisation de CURRENT_USER à partir d'un contexte d'emprunt  
Dans cet exemple, l’utilisateur `Wanida` exécute le code [!INCLUDE[tsql](../../includes/tsql-md.md)] suivant.
  
```sql
SELECT CURRENT_USER;  
GO  
EXECUTE AS USER = 'Wanida';  
GO  
SELECT CURRENT_USER;  
GO  
REVERT;  
GO  
SELECT CURRENT_USER;  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
Wanida
Arnalfo
Wanida
```
  
## <a name="see-also"></a>Voir aussi
[USER_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/user-name-transact-sql.md)  
[SYSTEM_USER &#40;Transact-SQL&#41;](../../t-sql/functions/system-user-transact-sql.md)  
[sys.database_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)  
[ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)  
[CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)  
[Fonctions système &#40;Transact-SQL&#41;](../../relational-databases/system-functions/system-functions-for-transact-sql.md)
  
  

