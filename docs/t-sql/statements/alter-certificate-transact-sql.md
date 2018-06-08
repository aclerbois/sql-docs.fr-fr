---
title: ALTER CERTIFICATE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/12/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: t-sql|statements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- ALTER_CERTIFICATE_TSQL
- ALTER CERTIFICATE
dev_langs:
- TSQL
helpviewer_keywords:
- ENCRYPTION BY PASSWORD option
- encryption [SQL Server], certificates
- modifying certificates
- private keys [SQL Server]
- ALTER CERTIFICATE statement
- certificates [SQL Server], modifying
ms.assetid: da4dc25e-72e0-4036-87ce-22de83160836
caps.latest.revision: 46
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: b478026b549078601540322e8f249cc718146425
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="alter-certificate-transact-sql"></a>ALTER CERTIFICATE (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  Modifie la clé privée utilisée pour chiffrer un certificat ou en ajoute une si aucune clé n'est présente. Affecte à un certificat la disponibilité [!INCLUDE[ssSB](../../includes/sssb-md.md)].  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
-- Syntax for SQL Server and Azure SQL Database  
  
ALTER CERTIFICATE certificate_name   
      REMOVE PRIVATE KEY  
    | WITH PRIVATE KEY ( <private_key_spec> [ ,... ] )  
    | WITH ACTIVE FOR BEGIN_DIALOG = [ ON | OFF ]  
  
<private_key_spec> ::=   
      FILE = 'path_to_private_key'   
    | DECRYPTION BY PASSWORD = 'key_password'   
    | ENCRYPTION BY PASSWORD = 'password'   
```  
  
```  
-- Syntax for Azure SQL Data Warehouse and Parallel Data Warehouse  
  
ALTER CERTIFICATE certificate_name   
{  
      REMOVE PRIVATE KEY  
    | WITH PRIVATE KEY (   
        FILE = '<path_to_private_key>',  
        DECRYPTION BY PASSWORD = '<key password>' )
}  
```  
  
## <a name="arguments"></a>Arguments  
 *certificate_name*  
 Nom unique sous lequel le certificat est connu dans la base de données.  
  
 FILE **='***path_to_private_key***'**  
 Spécifie le chemin d'accès complet, y compris le nom du fichier, à la clé privée. Ce paramètre peut être un chemin d'accès local ou un chemin d'accès UNC à un emplacement réseau. L'accès au fichier a lieu dans le contexte de sécurité du compte de service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Lorsque vous utilisez cette option, vous devez vérifier que le compte de service a accès au fichier spécifié.  
  
 DECRYPTION BY PASSWORD **='***key_password***'**  
 Spécifie le mot de passe exigé pour déchiffrer la clé privée.  
  
 ENCRYPTION BY PASSWORD **='***password***'**  
 Spécifie le mot de passe utilisé pour chiffrer la clé privée du certificat dans la base de données. *password* doit satisfaire aux critères de la stratégie de mot de passe Windows de l’ordinateur qui exécute l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d'informations, consultez [Password Policy](../../relational-databases/security/password-policy.md).  
  
 REMOVE PRIVATE KEY  
 Spécifie que la clé privée ne doit plus être conservée dans la base de données.  
  
 ACTIVE FOR BEGIN_DIALOG **=** { ON | OFF }  
 Met le certificat à disposition de l'initiateur d'une conversation [!INCLUDE[ssSB](../../includes/sssb-md.md)].  
  
## <a name="remarks"></a>Notes   
 La clé privée doit correspondre à la clé publique spécifiée par *certificate_name*.  
  
 Vous pouvez omettre la clause DECRYPTION BY PASSWORD si le mot de passe dans le fichier est protégé par un mot de passe vide.  
  
 Lorsque la clé privée d'un certificat qui existe déjà dans la base de données est importée à partir d'un fichier, elle est automatiquement protégée par la clé principale de la base de données. Pour protéger la clé privée avec un mot de passe, utilisez la clause ENCRYPTION BY PASSWORD.  
  
 L'option REMOVE PRIVATE KEY supprime de la base de données la clé privée du certificat. Vous pouvez faire cela lorsque le certificat sera utilisé pour vérifier des signatures ou dans des scénarios [!INCLUDE[ssSB](../../includes/sssb-md.md)] qui n'exigent pas une clé privée. Ne supprimez pas la clé privée d'un certificat qui protège une clé symétrique.  
  
 Il n'est pas nécessaire de spécifier un mot de passe de déchiffrement lorsque la clé privée est chiffrée à l'aide de la clé principale de la base de données.  
  
> [!IMPORTANT]  
>  Effectuez toujours une copie de la clé privée avant de la supprimer de la base de données. Pour plus d’informations, consultez [BACKUP CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/backup-certificate-transact-sql.md).  
  
 L'option WITH PRIVATE KEY n'est pas disponible dans une base de données à relation contenant-contenu.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation ALTER sur le certificat.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-changing-the-password-of-a-certificate"></a>A. Modification du mot de passe d'un certificat  
  
```  
ALTER CERTIFICATE Shipping04   
    WITH PRIVATE KEY (DECRYPTION BY PASSWORD = 'pGF$5DGvbd2439587y',  
    ENCRYPTION BY PASSWORD = '4-329578thlkajdshglXCSgf');  
GO  
```  
  
### <a name="b-changing-the-password-that-is-used-to-encrypt-the-private-key"></a>B. Modification du mot de passe utilisé pour chiffrer la clé privée  
  
```  
ALTER CERTIFICATE Shipping11   
    WITH PRIVATE KEY (ENCRYPTION BY PASSWORD = '34958tosdgfkh##38',  
    DECRYPTION BY PASSWORD = '95hkjdskghFDGGG4%');  
GO  
```  
  
### <a name="c-importing-a-private-key-for-a-certificate-that-is-already-present-in-the-database"></a>C. Importation d'une clé privée pour un certificat déjà présent dans la base de données  
  
```  
ALTER CERTIFICATE Shipping13   
    WITH PRIVATE KEY (FILE = 'c:\\importedkeys\Shipping13',  
    DECRYPTION BY PASSWORD = 'GDFLKl8^^GGG4000%');  
GO  
```  
  
### <a name="d-changing-the-protection-of-the-private-key-from-a-password-to-the-database-master-key"></a>D. Remplacement de la protection de la clé privée par mot de passe par une protection par clé principale de la base de données  
  
```  
ALTER CERTIFICATE Shipping15   
    WITH PRIVATE KEY (DECRYPTION BY PASSWORD = '95hk000eEnvjkjy#F%');  
GO  
```  
  
## <a name="see-also"></a> Voir aussi  
 [CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/create-certificate-transact-sql.md)   
 [DROP CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-certificate-transact-sql.md)   
 [BACKUP CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/backup-certificate-transact-sql.md)   
 [Hiérarchie de chiffrement](../../relational-databases/security/encryption/encryption-hierarchy.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)  
  
  

