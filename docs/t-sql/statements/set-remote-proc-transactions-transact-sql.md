---
title: SET REMOTE_PROC_TRANSACTIONS (Transact-SQL) | Microsoft Docs
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
- REMOTE_PROC_TRANSACTIONS_TSQL
- SET REMOTE_PROC_TRANSACTIONS
- REMOTE_PROC_TRANSACTIONS
- SET_REMOTE_PROC_TRANSACTIONS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- remote stored procedures [SQL Server]
- SET REMOTE_PROC_TRANSACTIONS statement
- distributed transactions [SQL Server], starting
- REMOTE_PROC_TRANSACTIONS option
- active transactions
ms.assetid: 4d284ae9-3f5f-465a-b0dd-1328a4832a03
caps.latest.revision: 37
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: cdd65007cf791c9467d71a22f0dd53eeae8f019f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="set-remoteproctransactions-transact-sql"></a>SET REMOTE_PROC_TRANSACTIONS (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Spécifie que lorsqu'une transaction locale est active, l'exécution d'une procédure stockée distante démarre une transaction distribuée [!INCLUDE[tsql](../../includes/tsql-md.md)] gérée par [!INCLUDE[msCoName](../../includes/msconame-md.md)] Distributed Transaction Coordinator (MS DTC).  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepNextDontUse](../../includes/ssnotedepnextdontuse-md.md)] Cette option est fournie pour la compatibilité descendante concernant les applications utilisant des procédures stockées distantes. Au lieu de publier des appels des procédures stockées distantes, utilisez des requêtes distribuées qui font référence à des serveurs liés. Ceux-ci sont définis au moyen de [sp_addlinkedserver](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md).  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
SET REMOTE_PROC_TRANSACTIONS { ON | OFF }   
```  
  
## <a name="arguments"></a>Arguments  
 ON | OFF  
 Si l'option est activée (ON), une transaction distribuée [!INCLUDE[tsql](../../includes/tsql-md.md)] est démarrée lorsqu'une procédure stockée distante est exécutée à partir d'une transaction locale. Si elle est désactivée, l'appel d'une procédure stockée distante depuis une transaction locale n'entraîne pas le démarrage d'une transaction distribuée [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
## <a name="remarks"></a>Notes   
 Si REMOTE_PROC_TRANSACTIONS est défini sur ON, l'appel d'une procédure stockée distante démarre une transaction distribuée et enregistre la transaction dans MS DTC. L'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] appelant la procédure stockée distante constitue l'élément créateur de la transaction et qui contrôle l'exécution jusqu'à son terme. Si une instruction COMMIT TRANSACTION ou ROLLBACK TRANSACTION est ensuite émise pour la connexion, le serveur de contrôle demande à MS DTC de gérer l'achèvement de la transaction distribuée sur tous les ordinateurs concernés.  
  
 Une fois la transaction distribuée [!INCLUDE[tsql](../../includes/tsql-md.md)] démarrée, des appels de procédures stockées distantes peuvent être émis vers d'autres instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui n'ont pas été définies en tant que serveurs distants. Les serveurs distants sont tous enregistrés dans la transaction distribuée [!INCLUDE[tsql](../../includes/tsql-md.md)] et MS DTC s'assure que la transaction est exécutée jusqu'à son terme sur chaque serveur distant.  
  
 REMOTE_PROC_TRANSACTIONS est un paramètre de connexion qui peut être utilisé pour remplacer l’option **sp_configure remote proc trans** au niveau de l’instance.  
  
 Lorsque REMOTE_PROC_TRANSACTIONS est défini sur OFF, les appels de procédures stockées distantes ne sont pas inclus dans une transaction locale. Les modifications effectuées par la procédure stockée distante sont validées ou annulées une fois celle-ci exécutée. Toute instruction COMMIT TRANSACTION ou ROLLBACK TRANSACTION ultérieure émise par la connexion ayant appelé la procédure stockée distante n'a aucun effet sur le traitement effectué par la procédure.  
  
 REMOTE_PROC_TRANSACTIONS est une option de compatibilité qui affecte uniquement les appels de procédures stockées distantes émis vers des instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] définies en tant que serveurs distants à l’aide de **sp_addserver**. Cette option ne s’applique pas aux requêtes distribuées qui exécutent une procédure stockée sur une instance définie en tant que serveur lié à l’aide de **sp_addlinkedserver**.  
  
 L'option SET REMOTE_PROC_TRANSACTIONS est définie lors de l'exécution, et non pas durant l'analyse.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle **public** .  
  
## <a name="see-also"></a> Voir aussi  
 [BEGIN DISTRIBUTED TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/begin-distributed-transaction-transact-sql.md)   
 [Instructions SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)  
  
  
