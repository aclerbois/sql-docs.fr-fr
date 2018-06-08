---
title: sysmail_help_principalprofile_sp (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 08/02/2016
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sysmail_help_principalprofile_sp_TSQL
- sysmail_help_principalprofile_sp
dev_langs:
- TSQL
helpviewer_keywords:
- sysmail_help_principalprofile_sp
ms.assetid: 0cfd6464-09c7-4f03-9d25-58001c096a9e
caps.latest.revision: 43
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: ca2ad195b8360a8e8963f95df4d0f0c517b907ce
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sysmailhelpprincipalprofilesp-transact-sql"></a>sysmail_help_principalprofile_sp (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Répertorie des informations sur les associations entre les profils de messagerie de la base de données et les principaux de la base de données.  
  
 
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sysmail_help_principalprofile_sp [ {   [ @principal_id = ] principal_id | [ @principal_name = ] 'principal_name' } ]  
    [ [ , ] {   [ @profile_id = ] profile_id | [ @profile_name = ] 'profile_name' } ]  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@principal_id=** ] *principal_id*  
 Est l’ID de l’utilisateur de base de données ou d’un rôle dans le **msdb** base de données pour l’association à répertorier. *principal_id* est **int**, avec NULL comme valeur par défaut. Soit *principal_id* ou *principal_name* peut être spécifié.  
  
 [  **@principal_name=** ] **'***principal_name***'**  
 Est le nom de l’utilisateur de base de données ou d’un rôle dans le **msdb** base de données pour l’association à répertorier. *principal_name* est **sysname**, avec NULL comme valeur par défaut. Soit *principal_id* ou *principal_name* peut être spécifié.  
  
 [  **@profile_id=** ] *profile_id*  
 Identificateur du profil pour l'association à répertorier. *profile_id* est **int**, avec NULL comme valeur par défaut. Soit *profile_id* ou *profile_name* peut être spécifié.  
  
 [  **@profile_name=** ] **'***profile_name***'**  
 Nom du profil pour l'association à répertorier. *profile_name* est **sysname**, avec NULL comme valeur par défaut. Soit *profile_id* ou *profile_name* peut être spécifié.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 **0** (réussite) ou **1** (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Retourne un jeu de résultats qui contient les colonnes répertoriées dans le tableau ci-dessous.  
  
||||  
|-|-|-|  
|Nom de colonne|Type de données| Description|  
|**principal_id**|**int**|Identificateur de l'utilisateur de la base de données.|  
|**principal_name**|**sysname**|Le nom de l’utilisateur de base de données.|  
|**profile_id**|**int**|Numéro d'identification du profil de messagerie de la base de données.|  
|**profile_name**|**sysname**|Nom du profil de messagerie de la base de données.|  
|**is_default**|**bit**|Indicateur signalant s'il s'agit du profil par défaut de l'utilisateur.|  
  
## <a name="remarks"></a>Notes  
 Si **sysmail_help_principalprofile_sp** est appelée sans paramètres, le jeu de résultats retourné répertorie toutes les associations de l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Sinon, l'ensemble de résultats contient des informations pour les associations qui correspondent aux paramètres fournis. Par exemple, la procédure répertorie toutes les associations d'un profil lorsque le nom de ce dernier est fourni.  
  
 **sysmail_help_principalprofile_sp** est dans le **msdb** de base de données et est détenue par le **dbo** schéma. La procédure doit être exécutée avec un nom en trois parties si la base de données actuelle n’est pas **msdb**.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle serveur fixe **sysadmin** .  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-listing-information-for-a-specific-association"></a>A. Affichage d'une liste d'informations pour une association spécifique  
 L'exemple suivant illustre l'affichage d'une liste d'informations sur toutes les associations entre le profil `AdventureWorks Administrator` et le principal `ApplicationLogin` de la base de données `msdb`.  
  
```  
EXECUTE msdb.dbo.sysmail_help_principalprofile_sp  
    @principal_name = 'danw',  
    @profile_name = 'AdventureWorks Administrator' ;  
```  
  
 Exemple d'un ensemble de résultats remis en forme au niveau de la longueur de ligne.  
  
```  
principal_id principal_name     profile_id  profile_name                   is_default  
------------ ------------------ ----------- ------------------------------ ----------  
5            danw               9           AdventureWorks Administrator   1  
```  
  
### <a name="b-listing-information-for-all-associations"></a>B. Affichage d'une liste d'informations pour toutes les associations  
 L'exemple suivant illustre l'affichage d'une liste d'informations sur toutes les associations de l'instance.  
  
```  
EXECUTE msdb.dbo.sysmail_help_principalprofile_sp ;  
```  
  
 Exemple d'un ensemble de résultats remis en forme au niveau de la longueur de ligne.  
  
```  
principal_id principal_name     profile_id  profile_name                   is_default  
------------ ------------------ ----------- ------------------------------ ----------  
6            terrid             3           Product Update Profile         1  
5            danw               9           AdventureWorks Administrator   1  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Messagerie de base de données](../../relational-databases/database-mail/database-mail.md)   
 [Procédures stockées de messagerie de base de données &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-mail-stored-procedures-transact-sql.md)  
  
  
