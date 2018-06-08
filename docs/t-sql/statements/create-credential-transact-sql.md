---
title: CREATE CREDENTIAL (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/09/2017
ms.prod: sql
ms.prod_service: sql-database
ms.component: t-sql|statements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- CREDENTIAL_TSQL
- SQL13.SWB.CREDENTIAL.GENERAL.F1
- CREATE CREDENTIAL
- CREATE_CREDENTIAL_TSQL
- CREDENTIAL
dev_langs:
- TSQL
helpviewer_keywords:
- SECRET clause
- authentication [SQL Server], credentials
- CREATE CREDENTIAL statement
- credentials [SQL Server], CREATE CREDENTIAL statement
ms.assetid: d5e9ae69-41d9-4e46-b13d-404b88a32d9d
caps.latest.revision: 51
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: = azuresqldb-mi-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: d1dbc4b6f4cf3dcb464f8e402bfd9d2579e63078
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34582021"
---
# <a name="create-credential-transact-sql"></a>CREATE CREDENTIAL (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdbmi-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdbmi-xxxx-xxx-md.md)]

  Crée des informations d’identification au niveau du serveur. Les informations d’identification sont un enregistrement qui contient les informations d’authentification requises pour la connexion à une ressource en dehors de SQL Server. La plupart des informations d'identification incluent un utilisateur et un mot de passe Windows. Par exemple, pour enregistrer une sauvegarde de base de données à un certain emplacement, il peut être nécessaire que SQL Server fournisse des informations d’identification spéciales afin d’accéder à cet emplacement. Pour plus d’informations, consultez [Informations d’identification (moteur de base de données)](../../relational-databases/security/authentication-access/credentials-database-engine.md).

[!INCLUDE[ssMIlimitation](../../includes/sql-db-mi-limitation.md)]

> [!NOTE]  
>  Pour créer les informations d’identification au niveau de la base de données, utilisez [CREATE DATABASE SCOPED CREDENTIAL &#40;Transact-SQL&#41;](../../t-sql/statements/create-database-scoped-credential-transact-sql.md). Utilisez des informations d’identification au niveau du serveur quand vous avez besoin d’utiliser les mêmes informations d’identification pour plusieurs bases de données sur le serveur. Utilisez des informations d’identification délimitées à la base de données afin d’accroître la portabilité de la base de données. Quand une base de données est déplacée vers un nouveau serveur, les informations d’identification délimitées à la base de données le sont également. Utilisez des informations d’identification délimitées à la base de données sur [!INCLUDE[ssSDS](../../includes/sssds-md.md)].  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
CREATE CREDENTIAL credential_name   
WITH IDENTITY = 'identity_name'  
    [ , SECRET = 'secret' ]  
        [ FOR CRYPTOGRAPHIC PROVIDER cryptographic_provider_name ]  
```  
  
## <a name="arguments"></a>Arguments  
 *credential_name*  
 Spécifie le nom des informations d'identification créées. *credential_name* ne peut pas commencer par le signe dièse (#). Les informations d'identification système commencent avec ##.  Quand vous utilisez une signature d’accès partagé (SAP), ce nom doit correspondre au chemin du conteneur, commencer par https, et ne pas contenir de barre oblique. Consultez l’exemple D ci-dessous.  
  
 IDENTITY **='***identity_name***'**  
 Spécifie le nom du compte à utiliser lors d'une connexion en dehors du serveur. Quand les informations d’identification sont utilisées pour accéder à Azure Key Vault, **IDENTITY** est le nom du coffre de clés. Consultez l'exemple C ci-dessous. Quand les informations d’identification utilisent une signature d’accès partagé, **identité** est *SHARED ACCESS SIGNATURE*. Consultez l’exemple D ci-dessous.  
  
 SECRET **='***secret***'**  
 Spécifie le secret requis pour l'authentification sortante.  
  
 Quand les informations d’identification sont utilisées pour accéder à Azure Key Vault, l’argument **SECRET** de **CREATE CREDENTIAL** exige que le *\<Client ID>* (sans tirets) et le *\<Secret>* d’un **principal du service** dans Azure Active Directory soient passés ensemble sans espace les séparant. Consultez l'exemple C ci-dessous. Quand les informations d’identification utilisent une signature d’accès partagé, **SECRET** est le jeton de signature d’accès partagé. Consultez l’exemple D ci-dessous.  Pour plus d’informations sur la création d’une stratégie d’accès stockée et d’une signature d’accès partagé sur un conteneur Azure, consultez [Leçon 1 : Créer une stratégie d’accès stockée et une signature d’accès partagé sur un conteneur Azure](../../relational-databases/lesson-1-create-stored-access-policy-and-shared-access-signature.md).  
  
 FOR CRYPTOGRAPHIC PROVIDER *cryptographic_provider_name*  
 Spécifie le nom d’un *Fournisseur EKM (Gestion de clés extensible)*. Pour plus d’informations sur la gestion des clés, consultez [Gestion de clés extensible &#40;EKM&#41;](../../relational-databases/security/encryption/extensible-key-management-ekm.md).  
  
## <a name="remarks"></a>Notes   

 Lorsque IDENTITY correspond à un utilisateur Windows, le secret peut être le mot de passe. Le secret est chiffré à l'aide de la clé principale de service. Si la clé principale de service est régénérée, le secret est chiffré de nouveau au moyen de la nouvelle clé principale de service.  
  
 Une fois les informations d’identification créées, vous pouvez les mapper à une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en utilisant [CREATE LOGIN](../../t-sql/statements/create-login-transact-sql.md) ou [ALTER LOGIN](../../t-sql/statements/alter-login-transact-sql.md). Une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut être mappée à un seul ensemble d’informations d’identification, mais des informations d’identification peuvent être mappées à plusieurs connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations, consultez [Informations d’identification &#40;moteur de base de données&#41;](../../relational-databases/security/authentication-access/credentials-database-engine.md). Des informations d’identification au niveau du serveur peuvent être mappées uniquement à une connexion, et pas à un utilisateur de base de données. 
  
 Des informations sur les informations d’identification sont consultables dans la vue de catalogue [sys.credentials](../../relational-databases/system-catalog-views/sys-credentials-transact-sql.md).  
  
 En l'absence d'une information d'identification mappée à une connexion pour le fournisseur, l'information d'identification mappée au compte de service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est utilisée.  
  
 Une connexion peut avoir plusieurs informations d'identification mappées à elle, à condition qu'elles soient utilisées avec des fournisseurs distinctifs. Il ne doit y avoir qu'une seule information d'identification mappée par fournisseur par connexion. La même information d'identification peut être mappée à d'autres connexions.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l’autorisation **ALTER ANY CREDENTIAL**.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-basic-example"></a>A. Exemple de base  
 L'exemple ci-dessous crée des informations d'identification nommées `AlterEgo`. Les informations d'identification contiennent l'utilisateur Windows `Mary5` et un mot de passe.  
  
```  
CREATE CREDENTIAL AlterEgo WITH IDENTITY = 'Mary5',   
    SECRET = '<EnterStrongPasswordHere>';  
GO  
```  
  
### <a name="b-creating-a-credential-for-ekm"></a>B. Création d'informations d'identification pour EKM  
 L'exemple suivant utilise un compte précédemment créé, `User1OnEKM`, sur un module EKM au moyen des outils d'administration EKM, avec un type de compte de base et un mot de passe. Le compte **sysadmin** sur le serveur crée des informations d’identification utilisées pour se connecter au compte EKM, et les attribue au compte `User1`[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] :  
  
```  
CREATE CREDENTIAL CredentialForEKM  
    WITH IDENTITY='User1OnEKM', SECRET='<EnterStrongPasswordHere>'  
    FOR CRYPTOGRAPHIC PROVIDER MyEKMProvider;  
GO  
  
/* Modify the login to assign the cryptographic provider credential */  
ALTER LOGIN Login1  
ADD CREDENTIAL CredentialForEKM;  
  
/* Modify the login to assign a non cryptographic provider credential */   
ALTER LOGIN Login1  
WITH CREDENTIAL = AlterEgo;  
GO  
```  
  
### <a name="c-creating-a-credential-for-ekm-using-the-azure-key-vault"></a>C. Création d'informations d'identification pour EKM à l'aide d’Azure Key Vault  
 L’exemple suivant crée des informations d’identification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilisables par le [!INCLUDE[ssDE](../../includes/ssde-md.md)] lors de l’accès à Azure Key Vault à l’aide du **connecteur SQL Server pour Microsoft Azure Key Vault**. Pour obtenir un exemple complet d’utilisation du connecteur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [Gestion de clés extensible à l’aide d’Azure Key Vault &#40;SQL Server&#41;](../../relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server.md).  
  
> [!IMPORTANT]  
>  L'argument **IDENTITY** de **CREATE CREDENTIAL** nécessite le nom du coffre de clés. Concernant l’argument **SECRET** de **CREATE CREDENTIAL**, *\<Client ID>* (sans tirets) et *\<Secret>* doivent être passés ensemble sans espace les séparant.  
  
 Dans l'exemple suivant, l' **ID client** (`EF5C8E09-4D2A-4A76-9998-D93440D8115D`) est débarrassé des tirets et entré comme chaîne `EF5C8E094D2A4A769998D93440D8115D` , tandis que la **clé secrète** est représentée par la chaîne *SECRET_DBEngine*.  
  
```  
USE master;  
CREATE CREDENTIAL Azure_EKM_TDE_cred   
    WITH IDENTITY = 'ContosoKeyVault',   
    SECRET = 'EF5C8E094D2A4A769998D93440D8115DSECRET_DBEngine'   
    FOR CRYPTOGRAPHIC PROVIDER AzureKeyVault_EKM_Prov ;  
```  
  
 L’exemple suivant crée les mêmes informations d’identification à l’aide de variables pour les chaînes **Client ID** et **Secret**, qui sont ensuite concaténées pour former l’argument **SECRET**. La fonction **REPLACE** est utilisée pour supprimer les tirets de l’ID client.  
  
```  
DECLARE @AuthClientId uniqueidentifier = 'EF5C8E09-4D2A-4A76-9998-D93440D8115D';  
DECLARE @AuthClientSecret varchar(200) = 'SECRET_DBEngine';  
DECLARE @pwd varchar(max) = REPLACE(CONVERT(varchar(36), @AuthClientId) , '-', '') + @AuthClientSecret;  
  
EXEC ('CREATE CREDENTIAL Azure_EKM_TDE_cred   
    WITH IDENTITY = 'ContosoKeyVault', SECRET = ''' + @PWD + '''   
    FOR CRYPTOGRAPHIC PROVIDER AzureKeyVault_EKM_Prov ;');  
```  
  
### <a name="d-creating-a-credential-using-a-sas-token"></a>D. Création d’informations d’identification à l’aide d’un jeton SAP  
 **S’applique à** : [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] jusqu’à la [version actuelle](http://go.microsoft.com/fwlink/p/?LinkId=299658).  
  
 L’exemple suivant crée des informations d’identification avec signature d’accès partagé à l’aide d’un jeton SAP.  Pour un tutoriel montrant comment créer une stratégie d’accès stockée et une signature d’accès partagé sur un conteneur Azure, puis créer des informations d’identification à l’aide de la signature d’accès partagé, consultez [Tutoriel : Utiliser le service Stockage Blob Azure avec SQL Server 2016](../../relational-databases/tutorial-use-azure-blob-storage-service-with-sql-server-2016.md).  
  
> [!IMPORTANT]  
>  L’argument **CREDENTIAL NAME** exige que le nom corresponde au chemin du conteneur, commence par https et ne se termine pas par une barre oblique. L’argument **IDENTITY** nécessite le nom, *SHARED ACCESS SIGNATURE*. L’argument **SECRET** nécessite le jeton de signature d’accès partagé.  
  
```  
USE master  
CREATE CREDENTIAL [https://<mystorageaccountname>.blob.core.windows.net/<mystorageaccountcontainername>] -- this name must match the container path, start with https and must not contain a trailing forward slash.  
   WITH IDENTITY='SHARED ACCESS SIGNATURE' -- this is a mandatory string and do not change it.   
   , SECRET = 'sharedaccesssignature' –- this is the shared access signature token   
GO    
```  
  
## <a name="see-also"></a> Voir aussi  
 [Informations d’identification &#40;moteur de base de données&#41;](../../relational-databases/security/authentication-access/credentials-database-engine.md)   
 [ALTER CREDENTIAL &#40;Transact-SQL&#41;](../../t-sql/statements/alter-credential-transact-sql.md)   
 [DROP CREDENTIAL &#40;Transact-SQL&#41;](../../t-sql/statements/drop-credential-transact-sql.md)   
 [CREATE DATABASE SCOPED CREDENTIAL &#40;Transact-SQL&#41;](../../t-sql/statements/create-database-scoped-credential-transact-sql.md)   
 [CREATE LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/create-login-transact-sql.md)   
 [ALTER LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/alter-login-transact-sql.md)   
 [sys.credentials &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-credentials-transact-sql.md)   
 [Leçon 2 : créer des informations d’identification SQL Server à l’aide d’une signature d’accès partagé](../../relational-databases/lesson-2-create-a-sql-server-credential-using-a-shared-access-signature.md)   
 [Signature d’accès partagé](https://azure.microsoft.com/en-us/documentation/articles/storage-dotnet-shared-access-signature-part-1/)  
  
  
