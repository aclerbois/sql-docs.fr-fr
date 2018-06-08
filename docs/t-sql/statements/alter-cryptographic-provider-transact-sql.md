---
title: ALTER CRYPTOGRAPHIC PROVIDER (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/20/2017
ms.prod: sql
ms.prod_service: sql-database
ms.component: t-sql|statements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- ALTER_CRYPTOGRAPHIC_PROVIDER_TSQL
- ALTER CRYPTOGRAPHIC PROVIDER
- ALTER_CRYPTOGRAPHIC_TSQL
- ALTER CRYPTOGRAPHIC
dev_langs:
- TSQL
helpviewer_keywords:
- ALTER CRYPTOGRAPHIC PROVIDER
ms.assetid: 876b6348-fb29-49e1-befc-4217979f6416
caps.latest.revision: 20
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: e7d99901080ae6b5d401f376c832f498ce9c5627
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="alter-cryptographic-provider-transact-sql"></a>ALTER CRYPTOGRAPHIC PROVIDER (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Modifie un fournisseur de services de chiffrement dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à partir d'un fournisseur EKM (Gestion de clés extensible).  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
ALTER CRYPTOGRAPHIC PROVIDER provider_name   
    [ FROM FILE = path_of_DLL ]  
    ENABLE | DISABLE  
```  
  
## <a name="arguments"></a>Arguments  
 *provider_name*  
 Nom du fournisseur EKM (Gestion de clés extensible).  
  
 *Path_of_DLL*  
 Chemin d'accès du fichier .dll qui implémente l'interface EKM (Gestion de clés extensible) [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ENABLE | DISABLE  
 Active ou désactive un fournisseur.  
  
## <a name="remarks"></a>Notes   
 Si le fournisseur modifie le fichier .dll utilisé pour implémenter la gestion de clés extensible dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vous devez utiliser l'instruction ALTER CRYPTOGRAPHIC PROVIDER.  
  
 Lorsque le chemin d'accès du fichier .dll est mis à jour à l'aide de l'instruction ALTER CRYPTOGRAPHIC PROVIDER, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] effectue les actions suivantes :  
-   Désactive le fournisseur.  
-   Vérifie la signature DLL et garantit que le fichier .dll possède le même GUID que celui qui est enregistré dans le catalogue.  
-   Met à jour la version DLL dans le catalogue.  
  

Lorsqu'un fournisseur de gestion de clés extensible a la valeur DISABLE, toute tentative d'utilisation du fournisseur avec des instructions de chiffrement échoue sur les nouvelles connexions.  
  
Pour désactiver un fournisseur, toutes les sessions qui utilisent le fournisseur doivent être terminées.  
  
Lorsqu'une DLL de fournisseur EKM n'implémente pas toutes les méthodes nécessaires, ALTER CRYPTOGRAPHIC PROVIDER peut retourner l'erreur 33085 :  
  
 `One or more methods cannot be found in cryptographic provider library '%.*ls'.`  
  
Lorsque le fichier d'en-tête utilisé pour créer la DLL de fournisseur EKM est obsolète, ALTER CRYPTOGRAPHIC PROVIDER peut retourner l'erreur 33032 :  
  
 `SQL Crypto API version '%02d.%02d' implemented by provider is not supported. Supported version is '%02d.%02d'.`  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation CONTROL sur le fournisseur de services de chiffrement.  
  
## <a name="examples"></a>Exemples  
 L’exemple suivant modifie un fournisseur de services de chiffrement, appelé `SecurityProvider` dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], en ajoutant une version plus récente d’un fichier .dll. Cette nouvelle version est nommée `c:\SecurityProvider\SecurityProvider_v2.dll` et est installée sur le serveur. Le certificat du fournisseur doit être installé sur le serveur.  
  
1. Désactivez le fournisseur pour effectuer la mise à niveau. Cela met fin à toutes les sessions de chiffrement ouvertes.  
```  
ALTER CRYPTOGRAPHIC PROVIDER SecurityProvider   
DISABLE;  
GO  
```  

2. Mettez à niveau le fichier .dll du fournisseur. Le GUID doit être identique à celui de la version précédente, mais la version peut être différente.  
```  
ALTER CRYPTOGRAPHIC PROVIDER SecurityProvider  
FROM FILE = 'c:\SecurityProvider\SecurityProvider_v2.dll';  
GO  
```  

3. Activez le fournisseur mis à niveau.   
```  
ALTER CRYPTOGRAPHIC PROVIDER SecurityProvider   
ENABLE;  
GO  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Gestion de clés extensible &#40;EKM&#41;](../../relational-databases/security/encryption/extensible-key-management-ekm.md)   
 [CREATE CRYPTOGRAPHIC PROVIDER &#40;Transact-SQL&#41;](../../t-sql/statements/create-cryptographic-provider-transact-sql.md)   
 [DROP CRYPTOGRAPHIC PROVIDER &#40;Transact-SQL&#41;](../../t-sql/statements/drop-cryptographic-provider-transact-sql.md)   
 [CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-symmetric-key-transact-sql.md)   
 [Gestion de clés extensible à l’aide d’Azure Key Vault &#40;SQL Server&#41;](../../relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server.md)  
  
  
