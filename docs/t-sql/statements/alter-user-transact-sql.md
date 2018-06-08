---
title: ALTER USER (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/05/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: t-sql|statements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- ALTER_USER_TSQL
- ALTER USER
dev_langs:
- TSQL
helpviewer_keywords:
- modifying database users
- ALTER USER statement
- renaming databases
- schemas [SQL Server], default
- database user names [SQL Server]
- names [SQL Server], database users
- default schemas
- modifying default schemas
ms.assetid: 344fc6ce-a008-47c8-a02e-47fae66cc590
caps.latest.revision: 75
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: aafcc77a26dcd75c5581f36acc8e37cd852684a6
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="alter-user-transact-sql"></a>ALTER USER (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  Renomme un utilisateur de base de données ou change son schéma par défaut.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
-- Syntax for SQL Server  
  
ALTER USER userName    
     WITH <set_item> [ ,...n ]  
[;]  
  
<set_item> ::=   
      NAME = newUserName   
    | DEFAULT_SCHEMA = { schemaName | NULL }  
    | LOGIN = loginName  
    | PASSWORD = 'password' [ OLD_PASSWORD = 'oldpassword' ]  
    | DEFAULT_LANGUAGE = { NONE | <lcid> | <language name> | <language alias> }  
    | ALLOW_ENCRYPTED_VALUE_MODIFICATIONS = [ ON | OFF ]  
```  
  
```  
-- Syntax for Azure SQL Database  
  
ALTER USER userName    
     WITH <set_item> [ ,...n ]  
  
<set_item> ::=   
      NAME = newUserName   
    | DEFAULT_SCHEMA = schemaName  
    | LOGIN = loginName  
    | ALLOW_ENCRYPTED_VALUE_MODIFICATIONS = [ ON | OFF ]   
[;]  
  
-- Azure SQL Database Update Syntax  
ALTER USER userName    
     WITH <set_item> [ ,...n ]  
[;]  
  
<set_item> ::=   
      NAME = newUserName   
    | DEFAULT_SCHEMA = { schemaName | NULL }  
    | LOGIN = loginName  
    | PASSWORD = 'password' [ OLD_PASSWORD = 'oldpassword' ]  
    | ALLOW_ENCRYPTED_VALUE_MODIFICATIONS = [ ON | OFF ]   
  
-- SQL Database syntax when connected to a federation member  
ALTER USER userName  
     WITH <set_item> [ ,… n ]   
[;]  
  
<set_item> ::=   
     NAME = newUserName  
```  
  
```  
-- Syntax for Azure SQL Data Warehouse and Parallel Data Warehouse  
  
ALTER USER userName    
     WITH <set_item> [ ,...n ]  
  
<set_item> ::=   
     NAME =newUserName   
     | LOGIN =loginName  
     | DEFAULT_SCHEMA = schema_name  
[;]  
```  
  
## <a name="arguments"></a>Arguments  
 *userName*  
 Spécifie le nom qui identifie l'utilisateur dans cette base de données.  
  
 LOGIN **=***loginName*  
 Remappe un utilisateur à une autre connexion en modifiant l'identificateur de sécurité (SID) de l'utilisateur de manière à ce qu'il corresponde au SID de la connexion.  
  
 Si l'instruction ALTER USER est la seule instruction d'un lot SQL, Microsoft Azure SQL Database prend en charge la clause WITH LOGIN. Si l'instruction ALTER USER n'est pas la seule instruction d'un lot SQL ou est exécutée en SQL dynamique, la clause WITH LOGIN n'est pas prise en charge.  
  
 NAME **=***newUserName*  
 Spécifie le nouveau nom de cet utilisateur. *newUserName* ne doit pas déjà exister dans la base de données active.  
  
 DEFAULT_SCHEMA **=** { *schemaName* | NULL }  
 Spécifie le premier schéma que le serveur va interroger pour résoudre les noms des objets associés à cet utilisateur. La définition du schéma par défaut sur NULL supprime un schéma par défaut d'un groupe Windows.   L'option NULL ne peut pas être utilisée avec un utilisateur Windows.  
  
 PASSWORD **=** '*password*'  
 **S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] jusqu’à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].  
  
 Spécifie le mot de passe de l'utilisateur à modifier. Les mots de passe respectent la casse.  
  
> [!NOTE]  
>  Cette option est uniquement disponible pour les utilisateurs à relation contenant-contenu. Pour plus d’informations, consultez [Bases de données à relation contenant-contenu](../../relational-databases/databases/contained-databases.md) et [sp_migrate_user_to_contained &#40;Transact-SQL&#41; ](../../relational-databases/system-stored-procedures/sp-migrate-user-to-contained-transact-sql.md).  
  
 OLD_PASSWORD **=***'oldpassword'*  
 **S’applique à** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] jusqu’à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].  
  
 Mot de passe de l’utilisateur actuel qui sera remplacé par '*password*'. Les mots de passe respectent la casse. *OLD_PASSWORD* est obligatoire pour changer un mot de passe, sauf si vous disposez de l’autorisation **ALTER ANY USER**. La spécification obligatoire du paramètre *OLD_PASSWORD* empêche les utilisateurs disposant de l’autorisation **IMPERSONATION** de changer le mot de passe.  
  
> [!NOTE]  
>  Cette option est uniquement disponible pour les utilisateurs à relation contenant-contenu.  
  
 DEFAULT_LANGUAGE **=***{ NONE | \<lcid> | \<language name> | \<language alias> }*  
 **S'applique à**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
 Spécifie une langue par défaut à affecter à l'utilisateur. Si cette option a la valeur NONE, la langue par défaut est la langue par défaut actuellement définie pour la base de données. Si la langue par défaut de la base de données est modifiée ultérieurement, la langue par défaut de l'utilisateur reste inchangée. *DEFAULT_LANGUAGE* peut être l’ID local (lcid), le nom de la langue ou l’alias de langue.  
  
> [!NOTE]  
>  Cette option peut être spécifiée uniquement dans une base de données à relation contenant-contenu et uniquement pour les utilisateurs à relation contenant-contenu.  
  
 ALLOW_ENCRYPTED_VALUE_MODIFICATIONS = [ ON | **OFF** ] ]  
 **S’applique à** : [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)] à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], [!INCLUDE[ssSDS](../../includes/sssds-md.md)].  
  
 Supprime les contrôles de métadonnées de chiffrement sur le serveur dans les opérations de copie en bloc. Cela permet à l’utilisateur de copier en bloc des données chiffrées entre des tables ou des bases de données, sans déchiffrer les données. La valeur par défaut est OFF.  
  
> [!WARNING]  
>  Une utilisation incorrecte de cette option peut entraîner une altération des données. Pour plus d’informations, consultez [Migrer des données sensibles protégées par Always Encrypted](../../relational-databases/security/encryption/migrate-sensitive-data-protected-by-always-encrypted.md).  
  
## <a name="remarks"></a>Notes   
 Le schéma par défaut correspond au premier schéma que le serveur doit interroger pour résoudre les noms des objets associés à cet utilisateur de base de données. Sauf spécification contraire, le schéma par défaut sera le propriétaire des objets créés par cet utilisateur de la base de données.  
  
 Si l'utilisateur possède un schéma par défaut, ce schéma par défaut est utilisé. Si l'utilisateur ne possède pas de schéma par défaut, mais qu'il est membre d'un groupe qui dispose d'un schéma par défaut, le schéma par défaut du groupe est utilisé. Si l'utilisateur ne possède pas de schéma par défaut, et qu'il est membre de plus d'un groupe, le schéma par défaut de l'utilisateur sera celui du groupe Windows avec le principal_id le plus bas et un schéma par défaut défini explicite. Si aucun schéma par défaut ne peut être déterminé pour un utilisateur, le schéma **dbo** est utilisé.  
  
 La valeur de DEFAULT_SCHEMA peut désigner un schéma qui n'existe pas encore dans la base de données. Vous pouvez donc affecter un schéma DEFAULT_SCHEMA à un utilisateur avant de créer le schéma en question.  
  
 En revanche, vous ne pouvez pas spécifier DEFAULT_SCHEMA pour un utilisateur mappé à un certificat ou à une clé asymétrique.  
  
> [!IMPORTANT]  
>  La valeur DEFAULT_SCHEMA est ignorée si l’utilisateur est membre du rôle serveur fixe **sysadmin**. Tous les membres du rôle serveur fixe **sysadmin** possèdent le schéma par défaut `dbo`.  
  
 Vous ne pouvez modifier le nom d'un utilisateur associé à une connexion d'accès ou à un groupe Windows que si le SID du nouveau nom d'utilisateur correspond au SID enregistré dans la base de données. Cette vérification permet d'empêcher l'usurpation des identités de connexion Windows dans la base de données.  
  
 La clause WITH LOGIN active le remappage d'un utilisateur à une connexion différente. Les utilisateurs sans connexion, ceux mappés à un certificat ou bien ceux mappés à une clé asymétrique ne peuvent pas être remappés avec cette clause. Seuls les utilisateurs SQL et les utilisateurs (ou groupes) Windows peuvent être remappés. La clause WITH LOGIN ne peut pas être utilisée pour modifier le type d'utilisateur, par exemple pour modifier un compte Windows en connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Le nom de l'utilisateur sera renommé automatiquement avec le nom de la connexion si les conditions suivantes sont remplies.  
  
-   L'utilisateur est un utilisateur Windows.  
  
-   Le nom est un nom Windows (contient une barre oblique inverse).  
  
-   Aucun nouveau nom n'a été spécifié.  
  
-   Le nom actuel diffère du nom de connexion.  
  
 Sinon, l'utilisateur ne sera pas renommé, sauf si l'appelant appelle également la clause NAME.  
  
Le nom d’un utilisateur mappé à un compte de connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], un certificat ou une clé asymétrique ne peut pas contenir de barre oblique inverse (\\).  
  
> [!CAUTION]  
>  [!INCLUDE[ssCautionUserSchema](../../includes/sscautionuserschema-md.md)]  
  
## <a name="security"></a>Sécurité  
  
> [!NOTE]  
>  Un utilisateur bénéficiant de l’autorisation **ALTER ANY USER** peut changer le schéma par défaut de n’importe quel utilisateur. Un utilisateur dont le schéma a été modifié peut, sans le savoir, sélectionner des données dans la mauvaise table ou exécuter du code à partir du mauvais schéma.  
  
### <a name="permissions"></a>Autorisations  
 Pour changer le nom d’un utilisateur, vous devez disposer de l’autorisation **ALTER ANY USER**.  
  
 Pour changer la connexion cible d’un utilisateur, vous devez disposer de l’autorisation **CONTROL** sur la base de données.  
  
 Pour changer le nom d’un utilisateur bénéficiant de l’autorisation **CONTROL** sur la base de données, vous devez disposer de l’autorisation **CONTROL** sur la base de données.  
  
 Pour changer le schéma ou la langue par défaut, vous devez disposer de l’autorisation **ALTER** sur l’utilisateur. Les utilisateurs peuvent modifier leur propre schéma ou langue par défaut.  
  
## <a name="examples"></a>Exemples  

Tous les exemples sont exécutés dans une base de données utilisateur.  

### <a name="a-changing-the-name-of-a-database-user"></a>A. Modification du nom d'un utilisateur de base de données  
 L'exemple suivant modifie le nom de l'utilisateur de base de données `Mary5` en `Mary51`.  
  
```  
ALTER USER Mary5 WITH NAME = Mary51;  
GO  
```  
  
### <a name="b-changing-the-default-schema-of-a-user"></a>B. Modification du schéma par défaut d'un utilisateur  
 L'exemple suivant modifie le schéma par défaut de l'utilisateur `Mary51` en `Purchasing`.  
  
```  
ALTER USER Mary51 WITH DEFAULT_SCHEMA = Purchasing;  
GO  
```  
  
### <a name="c-changing-several-options-at-once"></a>C. Modification de plusieurs options à la fois  
 L'exemple suivant modifie plusieurs options pour un utilisateur de base de données à relation contenant-contenu dans une instruction.  
  
**S'applique à**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
```  
ALTER USER Philip   
WITH  NAME = Philipe   
    , DEFAULT_SCHEMA = Development   
    , PASSWORD = 'W1r77TT98%ab@#’ OLD_PASSWORD = 'New Devel0per'   
    , DEFAULT_LANGUAGE  = French ;  
GO  
```  
  
  
## <a name="see-also"></a> Voir aussi  
 [CREATE USER &#40;Transact-SQL&#41;](../../t-sql/statements/create-user-transact-sql.md)   
 [DROP USER &#40;Transact-SQL&#41;](../../t-sql/statements/drop-user-transact-sql.md)   
 [Bases de données à relation contenant-contenu](../../relational-databases/databases/contained-databases.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)   
 [sp_migrate_user_to_contained &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-migrate-user-to-contained-transact-sql.md)  
  
  


