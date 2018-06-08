---
title: CREATE COLUMN MASTER KEY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/18/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: t-sql|statements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- SQL13.SWB.NEWCOLUMNMASTERKEYDEF.GENERAL.F1
- SQL13.SWB.COLUMNMASTERKEYDEF.GENERAL.F1
- CREATE COLUMN MASTER KEY
- COLUMN MASTER KEY
- CREATE_COLUMN_MASTER_KEY_TSQL
- COLUMN_MASTER_KEY_TSQL
- SQL13.SWB.NEWCOLUMNMASTERKEY.GENERAL.F1
- SQL13.SWB.COLUMNMASTERKEY.GENERAL.F1
dev_langs:
- TSQL
helpviewer_keywords:
- column master key definition
- column master key, create
- CREATE COLUMN MASTER KEY statement
- Always Encrypted, create column master key
ms.assetid: f8926b95-e146-4e3f-b56b-add0c0d0a30e
caps.latest.revision: 32
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: e5dafed981c030b5f06e41610fd3add6c4af0238
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-column-master-key-transact-sql"></a>CREATE COLUMN MASTER KEY (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2016-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2016-asdb-xxxx-xxx-md.md)]

  Crée un objet de métadonnées de clé principale de colonne dans une base de données. Une entrée de métadonnées de clé principale de colonne qui représente une clé, stockée dans un magasin de clés externe, qui sert à protéger (chiffrer) les clés de chiffrement de colonne quand vous utilisez la fonctionnalité [Always Encrypted &#40;moteur de base de données&#41;](../../relational-databases/security/encryption/always-encrypted-database-engine.md). Le fait d’avoir plusieurs clés principales de colonne autorise la rotation de clé, c’est-à-dire le changement périodique de la clé afin d’améliorer la sécurité. Vous pouvez créer une clé principale de colonne dans un magasin de clés et son objet de métadonnées correspondant dans la base de données à l’aide de l’Explorateur d’objets dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou PowerShell. Pour plus d’informations, consultez [Vue d’ensemble de la gestion des clés pour Always Encrypted](../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md).  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
CREATE COLUMN MASTER KEY key_name   
    WITH (  
        KEY_STORE_PROVIDER_NAME = 'key_store_provider_name',  
        KEY_PATH = 'key_path'   
         )   
[;]  
```  
  
## <a name="arguments"></a>Arguments  
 *key_name*  
 Nom sous lequel la clé principale de colonne sera connue dans la base de données.  
  
 *key_store_provider_name*  
 Spécifie le nom d’un fournisseur de magasin de clés principales de colonne, qui est un composant logiciel côté client qui encapsule un magasin de clés contenant la clé principale de colonne. Un pilote client compatible avec Always Encrypted utilise un nom de fournisseur de magasin de clés pour rechercher un fournisseur de magasin de clés dans le Registre de fournisseurs de magasin de clés du pilote. Le pilote utilise le fournisseur pour déchiffrer les clés de chiffrement de colonne, protégées par une clé principale de colonne, stockée dans le magasin de clés sous-jacent. Une valeur en texte brut de la clé de chiffrement de colonne est ensuite utilisée pour chiffrer les paramètres de requête, correspondant aux colonnes de base de données chiffrées, ou pour déchiffrer les résultats de requête à partir des colonnes chiffrées.  
  
 Les bibliothèques de pilotes clients compatibles avec Always Encrypted incluent des fournisseurs de magasin de clés pour les magasins de clés les plus populaires.   
  
Un ensemble de fournisseurs disponibles varie en fonction du type et de la version du pilote client. Pour plus d’informations sur des pilotes spécifiques, consultez la documentation Always Encrypted :

[Développer à l’aide d’Always Encrypted avec le Fournisseur de données .NET Framework pour SQL Server](../../relational-databases/security/encryption/develop-using-always-encrypted-with-net-framework-data-provider.md)


Les tableaux ci-dessous capturent les noms des fournisseurs de système :  
  
|Nom du fournisseur de magasin de clés|Magasin de clés sous-jacent|  
    |-----------------------------|--------------------------|
    |'MSSQL_CERTIFICATE_STORE'|Magasin de certificats Windows| 
    |'MSSQL_CSP_PROVIDER'|Un magasin, tel qu’un module de sécurité matériel (HSM), qui prend en charge Microsoft CryptoAPI.|
    |'MSSQL_CNG_STORE'|Un magasin, tel qu’un module de sécurité matériel (HSM), qui prend en charge l’API CNG (Cryptography Next Generation).|  
    |'Azure_Key_Vault'|Voir [Prise en main du coffre de clés Azure](https://azure.microsoft.com/documentation/articles/key-vault-get-started/).|  
  

 Vous pouvez implémenter un fournisseur de magasins de clés personnalisé pour stocker les clés principales de colonne dans un magasin pour lequel il n’existe aucun fournisseur de magasin de clés intégré dans votre pilote client compatible avec Always Encrypted.  Notez que les noms des fournisseurs de magasins de clés personnalisés ne peuvent pas commencer par 'MSSQL_', qui est un préfixe réservé pour les fournisseurs de magasins de clés [!INCLUDE[msCoName](../../includes/msconame-md.md)]. 

  
 key_path  
 Chemin de la clé dans le magasin de clés principales de colonne. Le chemin de clé doit être valide dans le contexte de chaque application cliente qui est censée chiffrer ou déchiffrer des données stockées dans une colonne protégée (indirectement) par la clé principale de colonne référencée, et l’application cliente doit être autorisée à accéder à la clé. Le format du chemin de clé est propre au fournisseur de magasin de clés. La liste suivante décrit le format des chemins de clés pour des fournisseurs de magasins de clés de système Microsoft particuliers.  
  
-   **Nom du fournisseur :** MSSQL_CERTIFICATE_STORE  
  
     **Format du chemin de clé :** *nom_magasin_certificats*/*emplacement_magasin_certificats*/*empreinte_certificat*  
  
     Où :  
  
     *emplacement_magasin_certificats*  
     Emplacement du magasin de certificats, qui doit être Current User ou Local Machine. Pour plus d’informations, consultez [Local Machine and Current User Certificate Stores](https://msdn.microsoft.com/library/windows/hardware/ff548653.aspx).  
  
     *nom_magasin_certificats*  
     Nom du magasin de certificats, par exemple 'My'.  
  
     *empreinte_certificat*  
     Empreinte du certificat.  
  
     **Exemples :**  
  
    ```  
    N'CurrentUser/My/BBF037EC4A133ADCA89FFAEC16CA5BFA8878FB94'  
  
    N'LocalMachine/My/CA5BFA8878FB94BBF037EC4A133ADCA89FFAEC16'  
    ```  
  
-   **Nom du fournisseur :** MSSQL_CSP_PROVIDER  
  
     **Format du chemin de clé :** *nom_fournisseur*/*identificateur_clé*  
  
     Où :  
  
     *ProviderName*  
     Nom d’un fournisseur de service de chiffrement, qui implémente CAPI, pour le magasin de clés principales de colonne. Si vous utilisez un module HSM comme magasin de clés, il doit s’agir du nom du fournisseur de service de chiffrement fourni par le fournisseur de votre module HSM. Le fournisseur doit être installé sur un ordinateur client.  
  
     *identificateur_clé*  
     Identificateur de la clé, utilisé comme clé principale de colonne, dans le magasin de clés.  
  
     **Exemples :**  
  
    ```  
    N'My HSM CSP Provider/AlwaysEncryptedKey1'  
    ```  
  
-   **Nom du fournisseur :** MSSQL_CNG_STORE  
  
     **Format du chemin de clé :** *nom_fournisseur*/*identificateur_clé*  
  
     Où :  
  
     *ProviderName*  
     Nom du fournisseur de stockage de clés (KSP), qui implémente l’API CNG (Cryptography Next Generation), pour le magasin de clés principales de colonne. Si vous utilisez un module HSM comme magasin de clés, il doit s’agir du nom du fournisseur de stockage de clés fourni par le fournisseur de votre module HSM. Le fournisseur doit être installé sur un ordinateur client.  
  
     *identificateur_clé*  
     Identificateur de la clé, utilisé comme clé principale de colonne, dans le magasin de clés.  
  
     **Exemples :**  
  
    ```  
    N'My HSM CNG Provider/AlwaysEncryptedKey1'  
    ```  

-   **Nom du fournisseur :** AZURE_KEY_STORE  
  
     **Format du chemin de clé :** *URL_clé*  
  
     Où :  
  
     *URL_clé*  
     URL de la clé dans Azure Key Vault.


Exemple :
 
`N'https://myvault.vault.azure.net:443/keys/MyCMK/4c05f1a41b12488f9cba2ea964b6a700'`  
  
## <a name="remarks"></a>Notes   

La création d’une entrée de métadonnées de clé principale de colonne est obligatoire avant qu’une entrée de métadonnées de clé de chiffrement de colonne puisse être créée dans la base de données et avant que toute colonne dans la base de données puisse être chiffrée à l’aide d’Always Encrypted. Notez qu’une entrée de clé principale de colonne dans les métadonnées ne contient pas la clé principale de colonne proprement dite, qui doit être stockée dans un magasin de clés de colonne externe (en dehors de SQL Server). Le nom du fournisseur de magasin de clés et le chemin de la clé principale de colonne dans les métadonnées doivent être valides pour qu’une application cliente puisse utiliser la clé principale de colonne pour déchiffrer une clé de chiffrement de colonne chiffrée avec la clé principale de colonne et pour interroger des colonnes chiffrées.


  
## <a name="permissions"></a>Autorisations  
 Nécessite l’autorisation **ALTER ANY COLUMN MASTER KEY**.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-creating-a-column-master-key"></a>A. Création d’une clé principale de colonne  
 Création d’une entrée de métadonnées de clé principale de colonne pour une clé principale de colonne stockée dans le magasin de certificats, pour des applications clientes qui utilisent le fournisseur MSSQL_CERTIFICATE_STORE afin d’accéder à la clé principale de colonne :  
  
```  
CREATE COLUMN MASTER KEY MyCMK  
WITH (  
     KEY_STORE_PROVIDER_NAME = N'MSSQL_CERTIFICATE_STORE',   
     KEY_PATH = 'Current User/Personal/f2260f28d909d21c642a3d8e0b45a830e79a1420'  
   );  
```  
  
 Création d’une entrée de métadonnées de clé principale de colonne pour une clé principale de colonne accessible par des applications clientes qui utilisent le fournisseur MSSQL_CNG_STORE :  
  
```  
CREATE COLUMN MASTER KEY MyCMK  
WITH (  
    KEY_STORE_PROVIDER_NAME = N'MSSQL_CNG_STORE',    
    KEY_PATH = N'My HSM CNG Provider/AlwaysEncryptedKey'  
);  
```  
  
 Création d’une clé principale de colonne stockée dans Azure Key Vault, pour des applications clientes qui utilisent le fournisseur AZURE_KEY_VAULT afin d’accéder à la clé principale de colonne :  
  
```  
CREATE COLUMN MASTER KEY MyCMK  
WITH (  
    KEY_STORE_PROVIDER_NAME = N'AZURE_KEY_VAULT',  
    KEY_PATH = N'https://myvault.vault.azure.net:443/keys/  
        MyCMK/4c05f1a41b12488f9cba2ea964b6a700');  
```  
  
 Création d’une clé principale de colonne stockée dans un magasin de clés principales de colonne personnalisé :  
  
```  
CREATE COLUMN MASTER KEY MyCMK  
WITH (  
    KEY_STORE_PROVIDER_NAME = 'CUSTOM_KEY_STORE',    
    KEY_PATH = 'https://contoso.vault/sales_db_tce_key'  
);  
```  
  
## <a name="see-also"></a> Voir aussi
 
* [DROP COLUMN MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-column-master-key-transact-sql.md)   
* [CREATE COLUMN ENCRYPTION KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-column-encryption-key-transact-sql.md)
* [sys.column_master_keys (Transact-SQL)](../../relational-databases/system-catalog-views/sys-column-master-keys-transact-sql.md)
* [Always Encrypted &#40;moteur de base de données&#41;](../../relational-databases/security/encryption/always-encrypted-database-engine.md)  
* [Vue d’ensemble de la gestion des clés pour Always Encrypted](../../relational-databases/security/encryption/overview-of-key-management-for-always-encrypted.md)
  
