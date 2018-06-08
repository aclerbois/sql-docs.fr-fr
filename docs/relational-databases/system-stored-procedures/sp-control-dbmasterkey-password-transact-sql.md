---
title: sp_control_dbmasterkey_password (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 02/25/2016
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_control_dbmasterkey_password
- sp_control_dbmasterkey_password_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_control_dbmasterkey_password
ms.assetid: 63979a87-42a2-446e-8e43-30481faaf3ca
caps.latest.revision: 27
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: a9894a10965affbd65406276445f6c84f05ce76e
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spcontroldbmasterkeypassword-transact-sql"></a>sp_control_dbmasterkey_password (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Permet d'ajouter et de supprimer des informations d'identification contenant le mot de passe requis pour ouvrir la clé principale d'une base de données.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_control_dbmasterkey_password @db_name = 'database_name,  
     @password = 'master_key_password' , @action = { 'add' | 'drop' }  
```  
  
## <a name="arguments"></a>Arguments  
 @db_name= N'*nom_base_de_données*'  
 Spécifie le nom de la base de données associée à ces informations d'identification. Il ne peut s'agir d'une base de données système. *database_name* est **nvarchar**.  
  
 @password= N'*mot de passe*'  
 Spécifie le mot de passe de la clé principale. *mot de passe* est **nvarchar**.  
  
 @action= N'add'  
 Indique que des informations d'identification seront ajoutées dans la banque d'informations d'identification pour la base de données spécifiée. Les informations d'identification contiennent le mot de passe de la clé principale de base de données. La valeur passée à @action est **nvarchar**.  
  
 @action= N'drop'  
 Indique que des informations d'identification seront supprimées de la banque d'informations d'identification pour la base de données spécifiée. La valeur passée à @action est **nvarchar**.  
  
## <a name="remarks"></a>Notes  
 Lorsque [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] requiert la clé principale d'une base de données pour chiffrer ou déchiffrer une clé, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tente de déchiffrer la clé principale de base de données à l'aide de la clé principale de service de l'instance. Si le déchiffrement échoue, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] recherche dans la banque d'informations d'identification les informations d'identification de clé principale qui possèdent le même GUID de famille que la base de données pour laquelle la clé principale est requise. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tente ensuite de déchiffrer la clé principale de base de données avec toutes les informations d'identification correspondantes, jusqu'à ce que le déchiffrement réussisse ou qu'il ne reste plus d'informations d'identification.  
  
> [!CAUTION]  
>  Ne créez pas d'informations d'identification de clé principale pour une base de données qui doit être inaccessible à sa et à d'autres principaux de serveur dotés de privilèges de haut niveau. Vous pouvez configurer une base de données de sorte que la hiérarchie de ses clés ne puisse pas être déchiffrée par la clé principale du service. Cette option est prise en charge dans le cadre d'une défense en profondeur de bases de données qui contiennent des données chiffrées qui ne doivent pas être accessibles à sa ni à d'autres principaux de serveur dotés de privilèges de haut niveau. La création d'informations d'identification de clé principale pour une telle base de données supprime cette défense en profondeur en permettant à sa et à d'autres principaux de serveur dotés de privilèges de haut niveau de déchiffrer la base de données.  
  
 Informations d’identification qui sont créées à l’aide de sp_control_dbmasterkey_password sont visibles dans le [sys.master_key_passwords](../../relational-databases/system-catalog-views/sys-master-key-passwords-transact-sql.md) affichage catalogue. Les noms des informations d'identification créées pour les clés principales des bases de données ont le format suivant :`##DBMKEY_<database_family_guid>_<random_password_guid>##`. Le mot de passe est stocké en tant que secret des informations d'identification. À chaque mot de passe ajouté dans la banque d'informations d'identification correspond une ligne dans sys.credentials.  
  
 Vous ne pouvez pas utiliser sp_control_dbmasterkey_password pour créer des informations d'identification pour les bases de données système suivantes : master, model, msdb ou tempdb.  
  
 sp_control_dbmasterkey_password ne vérifie pas que le mot de passe permet d'ouvrir la clé principale de la base de données spécifiée.  
  
 Si vous spécifiez un mot de passe déjà stocké dans des informations d'identification pour la base de données spécifiée, sp_control_dbmasterkey_password échoue.  
  
> [!NOTE]  
>  Deux bases de données issues d'instances de serveur différentes peuvent partager le même GUID de famille. Dans ce cas de figure, les bases de données partagent les mêmes enregistrements de clé principale dans la banque d'informations d'identification.  
  
 Les paramètres passés à sp_control_dbmasterkey_password n'apparaissent pas dans les traces.  
  
> [!NOTE]  
>  Lorsque vous utilisez les informations d'identification ajoutées à l'aide de sp_control_dbmasterkey_password pour ouvrir la clé principale de la base de données, celle-ci est rechiffrée par la clé principale du service. Si la base de données est en mode lecture seule, l'opération de rechiffrement échouera et la clé principale de la base de données restera non chiffrée. Pour un accès ultérieur à la clé principale de la base de données, vous devez utiliser l'instruction OPEN MASTER KEY et un mot de passe. Pour ne pas à avoir à utiliser un mot de passe, créez les informations d'identification avant de placer la base de données en mode lecture seule.  
  
 **Problème de compatibilité descendante potentiel :** actuellement, la procédure stockée ne vérifie pas si une clé principale existe. Cette opération est autorisée à des fins de compatibilité descendante, mais affiche un avertissement. Ce comportement est déconseillé. Dans une prochaine version de la clé principale doit exister et le mot de passe utilisé dans la procédure stockée **sp_control_dbmasterkey_password** doit être le même mot de passe comme l’un des mots de passe utilisés pour chiffrer la clé principale de base de données.  
  
## <a name="permissions"></a>Autorisations  
 Requiert l'autorisation CONTROL sur la base de données.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-creating-a-credential-for-the-adventureworks2012-master-key"></a>A. Création d'informations d'identification pour la clé principale d'AdventureWorks2012  
 Dans l'exemple ci-dessous, des informations d'identification sont créées pour la clé principale de la base de données `AdventureWorks2012` et le mot de passe de la clé principale est enregistré en tant que secret dans les informations d'identification. Étant donné que tous les paramètres qui sont passés à `sp_control_dbmasterkey_password` doit être de type de données **nvarchar**, les chaînes de texte sont converties avec l’opérateur de cast `N`.  
  
```  
EXEC sp_control_dbmasterkey_password @db_name = N'AdventureWorks2012',   
    @password = N'sdfjlkj#mM00sdfdsf98093258jJlfdk4', @action = N'add';  
GO  
```  
  
### <a name="b-dropping-a-credential-for-a-database-master-key"></a>B. Suppression d'informations d'identification pour la clé principale d'une base de données  
 Dans l'exemple ci-dessous, les informations d'identification créées dans l'exemple A sont supprimées. Notez que tous les paramètres sont requis, y compris le mot de passe.  
  
```  
EXEC sp_control_dbmasterkey_password @db_name = N'AdventureWorks2012',   
    @password = N'sdfjlkj#mM00sdfdsf98093258jJlfdk4', @action = N'drop';  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Configurer une base de données miroir chiffrée](../../database-engine/database-mirroring/set-up-an-encrypted-mirror-database.md)   
 [Procédures stockées de sécurité &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [sys.credentials &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-credentials-transact-sql.md)   
 [Informations d’identification &#40;moteur de base de données&#41;](../../relational-databases/security/authentication-access/credentials-database-engine.md)  
  
  
