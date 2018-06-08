---
title: sp_certify_removable (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_certify_removable_TSQL
- sp_certify_removable
dev_langs:
- TSQL
helpviewer_keywords:
- sp_certify_removable
ms.assetid: ca12767f-0ae5-4652-b523-c23473f100a1
caps.latest.revision: 26
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: ba9ff14bc26b18eaf80dff000f141502a01fcc7b
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spcertifyremovable-transact-sql"></a>sp_certify_removable (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Vérifie qu'une base de données est correctement configurée pour la distribution sur support de sauvegarde amovible et rend compte à l'utilisateur des éventuels problèmes.  
  
> **IMPORTANT** [! INCLURE[ssNoteDepFutureAvoid](../../t-sql/statements/create-database-sql-server-transact-sql.md) à la place.  
  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_certify_removable [ @dbname= ] 'dbname'  
     [ , [ @autofix = ] 'auto' ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@dbname=**] **'***dbname***'**  
 Spécifie la base de données à vérifier. *dbname* est **sysname**.  
  
 [  **@autofix=**] **'auto'**  
 Donne la propriété de la base de données et de tous les objets de base de données à l'administrateur système, et supprime les utilisateurs de base de données créés par l'utilisateur et les autorisations autres que celles par défaut. *Auto* est **nvarchar (4)**, avec NULL comme valeur par défaut.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="remarks"></a>Notes  
 Si la base de données est correctement configurée, **sp_certify_removable** exécute les actions suivantes :  
  
-   Elle met la base de données hors ligne pour permettre la copie des fichiers.  
  
-   Elle met à jour les statistiques de toutes les tables et rend compte des éventuels problèmes de propriété et d'utilisateur.  
  
-   Elle marque également les groupes de fichiers de données en lecture seule pour que ces fichiers puissent être copiés sur des supports en lecture seule.  
  
 L'administrateur système doit être le propriétaire de la base de données et de tous les objets de la base de données. L’administrateur système est un utilisateur connu qui existe sur tous les serveurs qui exécutent [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et peut être augmentée à exister lors de la base de données de distribution ultérieurement et installé.  
  
 Si vous exécutez **sp_certify_removable** sans le **automatique** valeur et il retourne des informations sur une des conditions suivantes :  
  
-   L'administrateur système n'est pas le propriétaire de la base de données.  
  
-   Il existe des utilisateurs créés par l'utilisateur.  
  
-   L'administrateur système ne possède pas tous les objets de la base de données.  
  
-   Des autorisations non établies par défaut ont été accordées.  
  
 Vous pouvez y remédier des deux manières suivantes :  
  
-   Utilisez [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] outils et procédures, puis exécutez **sp_certify_removable** à nouveau.  
  
-   Il suffit d’exécuter **sp_certify_removable** avec la **automatique** valeur.  
  
 Remarquez que cette procédure stockée ne recherche que les utilisateurs et les autorisations des utilisateurs. Vous pouvez ajouter des groupes à la base de données et leur accorder des autorisations. Pour plus d’informations, consultez [GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-transact-sql.md).  
  
## <a name="permissions"></a>Autorisations  
 Exécuter les autorisations sont limitées aux membres de la **sysadmin** rôle serveur fixe.  
  
## <a name="examples"></a>Exemples  
 Cet exemple certifie que la base de données `inventory` est prête à être supprimée.  
  
```  
EXEC sp_certify_removable inventory, AUTO;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Attacher et détacher une base de données &#40;SQL Server&#41;](../../relational-databases/databases/database-detach-and-attach-sql-server.md)   
 [sp_create_removable &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-create-removable-transact-sql.md)   
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [sp_dbremove &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dbremove-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
