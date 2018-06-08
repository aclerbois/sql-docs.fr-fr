---
title: CREATE CRYPTOGRAPHIC PROVIDER (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.component: t-sql|statements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- CREATE_CRYPTOGRAPHIC_TSQL
- CRYPTOGRAPHIC PROVIDER
- PROVIDER_TSQL
- CREATE CRYPTOGRAPHIC
- CREATE CRYPTOGRAPHIC PROVIDER
- CRYPTOGRAPHIC_PROVIDER_TSQL
- CREATE_CRYPTOGRAPHIC_PROVIDER_TSQL
- PROVIDER
dev_langs:
- TSQL
helpviewer_keywords:
- 33085 (Database Engine error)
- CREATE CRYPTOGRAPHIC PROVIDER statement
- 33032 (Database Engine error)
ms.assetid: 059a39a6-9d32-4d3f-965b-0a1ce75229c7
caps.latest.revision: 20
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 6aae0136e9daa4c6b39e613cc5b0455308c907e2
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-cryptographic-provider-transact-sql"></a>CREATE CRYPTOGRAPHIC PROVIDER (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Crée un fournisseur de services de chiffrement dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à partir d'un fournisseur EKM (Extensible Key Management).  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
CREATE CRYPTOGRAPHIC PROVIDER provider_name   
    FROM FILE = path_of_DLL  
```  
  
## <a name="arguments"></a>Arguments  
 *provider_name*  
 Nom du fournisseur EKM (Extensible Key Management)  
  
 *path_of_DLL*  
 Chemin d'accès du fichier .dll qui implémente l'interface [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] EKM. Quand vous utilisez le **connecteur SQL Server pour Microsoft Azure Key Vault** l’emplacement par défaut est **'C:\Program Files\Microsoft SQL Server Connector for Microsoft Azure Key Vault\Microsoft.AzureKeyVaultService.EKM.dll'**.  
  
## <a name="remarks"></a>Notes   
 Toutes les clés créées par un fournisseur référenceront ce dernier par son GUID. Le GUID est conservé dans toutes les versions de la DLL.  
  
 La DLL qui implémente l'interface SQLEKM doit être signée numériquement à l'aide de n'importe quel certificat. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vérifiera la signature. La vérification inclut sa chaîne de certificats, dont la racine doit être installée à l’emplacement **Autorités de certification racines de confiance** sur un système Windows. Si la signature n’est pas vérifiée correctement, l’instruction CREATE CRYPTOGRAPHIC PROVIDER échoue. Pour plus d’informations sur les certificats et les chaînes de certificats, consultez [Certificats et clés asymétriques SQL Server](../../relational-databases/security/sql-server-certificates-and-asymmetric-keys.md).  
  
 Lorsqu'une DLL de fournisseur EKM n'implémente pas toutes les méthodes nécessaires, CREATE CRYPTOGRAPHIC PROVIDER peut retourner l'erreur 33085 :  
  
 `One or more methods cannot be found in cryptographic provider library '%.*ls'.`  
  
 Lorsque le fichier d'en-tête utilisé pour créer la DLL de fournisseur EKM est obsolète, CREATE CRYPTOGRAPHIC PROVIDER peut retourner l'erreur 33032 :  
  
 `SQL Crypto API version '%02d.%02d' implemented by provider is not supported. Supported version is '%02d.%02d'.`  
  
## <a name="permissions"></a>Autorisations  
 Requiert l’autorisation CONTROL SERVER ou l’appartenance au rôle serveur fixe **sysadmin**.  
  
## <a name="examples"></a>Exemples  
 L’exemple suivant crée un fournisseur de services de chiffrement appelé `SecurityProvider` dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à partir d’un fichier .dll. Le fichier .dll est nommé `c:\SecurityProvider\SecurityProvider_v1.dll` et installé sur le serveur. Le certificat du fournisseur doit d'abord être installé sur le serveur.  
  
```  
-- Install the provider  
CREATE CRYPTOGRAPHIC PROVIDER SecurityProvider  
    FROM FILE = 'C:\SecurityProvider\SecurityProvider_v1.dll';  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Gestion de clés extensible &#40;EKM&#41;](../../relational-databases/security/encryption/extensible-key-management-ekm.md)   
 [ALTER CRYPTOGRAPHIC PROVIDER &#40;Transact-SQL&#41;](../../t-sql/statements/alter-cryptographic-provider-transact-sql.md)   
 [DROP CRYPTOGRAPHIC PROVIDER &#40;Transact-SQL&#41;](../../t-sql/statements/drop-cryptographic-provider-transact-sql.md)   
 [CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-symmetric-key-transact-sql.md)   
 [Gestion de clés extensible à l’aide d’Azure Key Vault &#40;SQL Server&#41;](../../relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server.md)  
  
  
