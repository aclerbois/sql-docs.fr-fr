---
title: sp_addapprole (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_addapprole_TSQL
- sp_addapprole
dev_langs:
- TSQL
helpviewer_keywords:
- sp_addapprole
ms.assetid: 24200295-9a54-4cab-9922-fb2e88632721
caps.latest.revision: 31
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 3de15b3d1f02f3acf8dd5809dce7ec01ad74e65f
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spaddapprole-transact-sql"></a>sp_addapprole (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Ajoute un rôle d'application à la base de données active.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Utilisez [CREATE APPLICATION ROLE](../../t-sql/statements/create-application-role-transact-sql.md) à la place.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_addapprole [ @rolename = ] 'role' , [ @password = ] 'password'  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@rolename =** ] **'***rôle***'**  
 Nom du nouveau rôle d'application. *rôle* est **sysname**, sans valeur par défaut. *rôle* doit être un identificateur valid et ne peut pas déjà exister dans la base de données actuelle.  
  
 Les rôles d'application peuvent contenir de 1 à 128 caractères, y compris les lettres, les symboles et les nombres. Les noms de rôle ne peut pas contenir une barre oblique inverse (\\), ni être NULL ou une chaîne vide («).  
  
 [  **@password =** ] **'***mot de passe***'**  
 Mot de passe nécessaire pour activer le rôle d'application. *mot de passe* est **sysname**, sans valeur par défaut. *mot de passe* ne peut pas être NULL.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="remarks"></a>Notes  
 Dans les versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], les utilisateurs (et leurs rôles) ne sont pas complètement distincts des schémas. À compter de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)], les schémas sont complètement distincts des rôles. Cette nouvelle architecture se manifeste dans le comportement de CREATE APPLICATION ROLE. Cette instruction remplace **sp_addapprole**.  
  
 Pour assurer la compatibilité descendante avec les versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], **sp_addapprole** procède comme suit :  
  
-   S'il n'existe pas encore de schéma portant le même nom que le rôle d'application, ce schéma est créé. Le nouveau schéma est la propriété du rôle d'application et devient le schéma par défaut de ce rôle.  
  
-   S'il existe déjà un schéma portant le même nom que le rôle d'application, la procédure échoue.  
  
-   La complexité du mot de passe n’est pas vérifiée par **sp_addapprole**. contrairement à CREATE APPLICATION ROLE.  
  
 Le paramètre *mot de passe* est stocké comme un hachage unidirectionnel.  
  
 Le **sp_addapprole** procédure stockée ne peut pas être exécutée à partir d’une transaction définie par l’utilisateur.  
  
> [!IMPORTANT]  
>  Microsoft ODBC **chiffrer** option n’est pas pris en charge par **SqlClient**. Invitez si possible les utilisateurs à entrer les informations d'identification des rôles d'application au moment de l'exécution. Évitez de stocker ces informations dans un fichier. Si vous devez les conserver, chiffrez-les à l'aide des fonctions CryptoAPI.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation ALTER ANY APPLICATION ROLE sur la base de données. S'il n'existe pas encore de schéma présentant le même nom et le même propriétaire que le nouveau rôle, l'autorisation CREATE SCHEMA sur la base de données est également requise.  
  
## <a name="examples"></a>Exemples  
 L’exemple suivant ajoute le nouveau rôle d’application `SalesApp` avec le mot de passe `x97898jLJfcooFUYLKm387gf3` à la base de données actuelle.  
  
```  
EXEC sp_addapprole 'SalesApp', 'x97898jLJfcooFUYLKm387gf3' ;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [CREATE APPLICATION ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-application-role-transact-sql.md)  
  
  
