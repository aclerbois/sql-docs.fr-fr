---
title: SETUSER (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.prod_service: sql-database
ms.component: t-sql|statements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- SETUSER_TSQL
- SETUSER
dev_langs:
- TSQL
helpviewer_keywords:
- delegation [SQL Server]
- impersonation [SQL Server]
- SETUSER statement
- user impersonation [SQL Server]
ms.assetid: 7acfac5c-9ad6-4226-b874-7add36c4ea43
caps.latest.revision: 27
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: a592e2a335e8ce04341dba4a7bbc160f84b4b68a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="setuser-transact-sql"></a>SETUSER (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Permet à un membre du rôle serveur fixe **sysadmin** ou le propriétaire d’une base de données d’emprunter l’identité d’un autre utilisateur.  
  
> [!IMPORTANT]  
>  L'instruction SETUSER est incluse pour des raisons de compatibilité descendante uniquement. Elle ne sera peut-être plus prise en charge dans une version future de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Nous vous recommandons d’utiliser [EXECUTE AS](../../t-sql/statements/execute-as-transact-sql.md) à la place.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
SETUSER [ 'username' [ WITH NORESET ] ]   
```  
  
## <a name="arguments"></a>Arguments  
 **'** *username* **'**  
 Nom d'un utilisateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou Windows dans la base de données active dont l'identité est empruntée. Quand aucun *username* n’est spécifié, l’identité d’origine de l’administrateur système ou du propriétaire de la base de données qui emprunte l’identité de l’utilisateur est réinitialisée.  
  
 WITH NORESET  
 Spécifie que les instructions SETUSER suivantes (sans *username* spécifié) ne doivent pas réinitialiser l’identité de l’utilisateur avec l’administrateur système ou le propriétaire de la base de données.  
  
## <a name="remarks"></a>Notes   
 L’instruction SETUSER peut être utilisée par un membre du rôle serveur fixe **sysadmin** ou le propriétaire de la base de données pour tester les autorisations d’un autre utilisateur en adoptant l’identité de ce dernier. L’appartenance au rôle de base de données fixe db_owner ne suffit pas.  
  
 Utilisez SETUSER uniquement avec des utilisateurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. SETUSER n'est pas prise en charge avec les utilisateurs Windows. Lorsque l'instruction SETUSER est exécutée pour représenter un autre utilisateur, tout objet créé par l'utilisateur qui emprunte l'identité est la propriété de l'utilisateur représenté. Par exemple, si le propriétaire de la base de données utilise l’identité de l’utilisateur **Margaret** et crée une table appelée **orders**, la table **orders** est la propriété de **Margaret**, et non celle de l’administrateur système.  
  
 L'instruction SETUSER reste active jusqu'à ce qu'une autre instruction SETUSER soit exécutée ou jusqu'à ce que l'on change de base de données active à l'aide de l'instruction USE.  
  
> [!NOTE]  
>  Si l'instruction SETUSER WITH NORESET est exécutée, le propriétaire de la base de données ou l'administrateur système doit rétablir ses propres droits en se déconnectant, puis en se connectant de nouveau.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l’appartenance au rôle serveur fixe **sysadmin** ou doit être le propriétaire de la base de données. L’appartenance au rôle de base de données fixe **db_owner** ne suffit pas.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant montre comment le propriétaire d'une base de données peut prendre l'identité d'un autre utilisateur. L'utilisateur `mary` a créé une table appelée `computer_types`. À l'aide de l'instruction SETUSER, le propriétaire de la base de données représente `mary` pour autoriser l'utilisateur `joe` à accéder à la table `computer_types`, puis rétablit sa propre identité.  
  
```  
SETUSER 'mary';  
GO  
GRANT SELECT ON computer_types TO joe;  
GO  
--To revert to the original user  
SETUSER;  
```  
  
## <a name="see-also"></a> Voir aussi  
 [DENY &#40;Transact-SQL&#41;](../../t-sql/statements/deny-transact-sql.md)   
 [GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-transact-sql.md)   
 [REVOKE &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-transact-sql.md)   
 [USE &#40;Transact-SQL&#41;](../../t-sql/language-elements/use-transact-sql.md)  
  
  
