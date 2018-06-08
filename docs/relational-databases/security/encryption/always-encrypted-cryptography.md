---
title: Chiffrement Always Encrypted
ms.custom: ''
ms.date: 02/29/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: security
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- Always Encrypted, cryptography system
ms.assetid: ae8226ff-0853-4716-be7b-673ce77dd370
caps.latest.revision: 11
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: ead5689c2edb47f4ce2699e6b94bff53957ce9fd
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="always-encrypted-cryptography"></a>Chiffrement Always Encrypted
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]

  Ce document décrit les algorithmes et mécanismes de chiffrement permettant de dériver le matériel de chiffrement utilisé dans la fonctionnalité [Always Encrypted](../../../relational-databases/security/encryption/always-encrypted-database-engine.md) dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] et [!INCLUDE[ssSDSFull](../../../includes/sssdsfull-md.md)].  
  
## <a name="keys-key-stores-and-key-encryption-algorithms"></a>Clés, magasins de clé et algorithmes de chiffrement par clé  
 Always Encrypted utilise deux types de clés : des clés principales de colonne et des clés de chiffrement de colonne.  
  
 Une clé principale de colonne (CMK) est une clé de chiffrement de clé (par exemple, une clé utilisée pour chiffrer d'autres clés) qui est toujours contrôlée par le client et stockée dans un magasin de clés externe. Un pilote client avec Always Encrypted interagit avec le magasin de clés via un fournisseur de magasins CMK, qui peut faire partie de la bibliothèque de pilotes (un fournisseur [!INCLUDE[msCoName](../../../includes/msconame-md.md)]/système) ou de l’application cliente (un fournisseur personnalisé). Les bibliothèques de pilotes clients incluent actuellement des fournisseurs de magasins de clés [!INCLUDE[msCoName](../../../includes/msconame-md.md)] pour le [magasin de certificats Windows](https://msdn.microsoft.com/library/windows/desktop/aa388160) et les modules de sécurité matériels (HSM).  (Pour obtenir la liste des fournisseurs, consultez [CREATE COLUMN MASTER KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/create-column-master-key-transact-sql.md).) Un développeur d'applications peut proposer un fournisseur personnalisé pour un magasin arbitraire.  
  
 Une clé de chiffrement de colonne (CEK) est une clé de chiffrement de contenu (par exemple, une clé utilisée pour protéger les données) protégée par une clé CMK.  
  
 Tous les fournisseurs de magasins de clés CMK [!INCLUDE[msCoName](../../../includes/msconame-md.md)] chiffrent les clés CEK à l’aide de la méthode RSA-OAEP (RSA with Optimal Asymmetric Encryption) avec les paramètres par défaut spécifiés dans l’article RFC 3447, section A.2.1. Ces paramètres par défaut utilisent une fonction de hachage SHA-1 et une fonction de génération de masque MGF1 avec SHA-1.  
  
## <a name="data-encryption-algorithm"></a>Algorithme de chiffrement des données  
 Always Encrypted utilise l’algorithme **AEAD_AES_256_CBC_HMAC_SHA_256** pour chiffrer les données de la base de données.  
  
 **AEAD_AES_256_CBC_HMAC_SHA_256** est dérivé du projet de spécification disponible ici : [http://tools.ietf.org/html/draft-mcgrew-aead-aes-cbc-hmac-sha2-05](http://tools.ietf.org/html/draft-mcgrew-aead-aes-cbc-hmac-sha2-05). Il utilise un schéma de chiffrement authentifié avec données associées, en suivant une approche Encrypt-then-MAC. Autrement dit, le texte en clair est d'abord chiffré puis les informations MAC sont générées selon le texte chiffré qui en résulte.  
  
 Pour masquer les modèles, **AEAD_AES_256_CBC_HMAC_SHA_256** utilise le mode CBC (Cipher Block Chaining), dans lequel une valeur initiale est chargée dans le système nommé vecteur d’initialisation (IV). Vous trouverez la description complète du mode CBC ici : [http://csrc.nist.gov/publications/nistpubs/800-38a/sp800-38a.pdf](http://csrc.nist.gov/publications/nistpubs/800-38a/sp800-38a.pdf).  
  
 **AEAD_AES_256_CBC_HMAC_SHA_256** calcule une valeur de texte chiffré pour une valeur en texte clair donnée en procédant comme suit.  
  
### <a name="step-1-generating-the-initialization-vector-iv"></a>Étape 1: création du vecteur d'initialisation (IV)  
 Always Encrypted prend en charge deux variantes de **AEAD_AES_256_CBC_HMAC_SHA_256**:  
  
-   Aléatoire  
  
-   Déterministe  
  
 Pour le chiffrement aléatoire, le vecteur d'initialisation est généré de façon aléatoire. Par conséquent, chaque fois que le même texte en clair est chiffré, un texte chiffré différent est généré, ce qui empêche toute divulgation d'informations.  
  
```  
When using randomized encryption: IV = Generate cryptographicaly random 128bits  
```  
  
 Dans le cas du chiffrement déterministe, le vecteur d'initialisation n'est pas généré de façon aléatoire mais plutôt dérivé de la valeur de texte en clair à l'aide de l'algorithme suivant :  
  
```  
When using deterministic encryption: IV = HMAC-SHA-256( iv_key, cell_data ) truncated to 128 bits.  
```  
  
 Où la valeur iv_key est dérivée de la clé CEK de la façon suivante :  
  
```  
iv_key = HMAC-SHA-256(CEK, "Microsoft SQL Server cell IV key" + algorithm + CEK_length)  
```  
  
 La troncation de la valeur HMAC est effectuée afin d'ajuster 1 bloc de données si nécessaire pour le vecteur d'initialisation.    
Par conséquent, le chiffrement déterministe génère toujours le même texte chiffré pour une valeur en texte clair donnée, ce qui permet de déduire si deux valeurs de texte en clair sont égales en comparant leurs valeurs de texte chiffré correspondantes. Cette divulgation limitée des informations permet au système de base de données de prendre en charge une comparaison d'égalité au niveau des valeurs de colonne chiffrées.  
  
 Le chiffrement déterministe est plus efficace pour dissimuler les modèles par rapport aux autres méthodes comme l'utilisation d'une valeur IV prédéfinie.  
  
### <a name="step-2-computing-aes256cbc-ciphertext"></a>Étape 2 : calcul du texte chiffré AES_256_CBC  
 Après le calcul du vecteur d’initialisation, le texte chiffré **AES_256_CBC** est généré :  
  
```  
aes_256_cbc_ciphertext = AES-CBC-256(enc_key, IV, cell_data) with PKCS7 padding.  
```  
  
 Où la clé de chiffrement (enc_key) est dérivée de la clé CEK comme suit.  
  
```  
enc_key = HMAC-SHA-256(CEK, "Microsoft SQL Server cell encryption key" + algorithm + CEK_length )  
```  
  
### <a name="step-3-computing-mac"></a>Étape 3 : calcul de la valeur MAC  
 Par la suite, la valeur MAC est calculée à l'aide de l'algorithme suivant :  
  
```  
MAC = HMAC-SHA-256(mac_key, versionbyte + IV + Ciphertext + versionbyte_length)  
```  
  
 Où :  
  
```  
versionbyte = 0x01 and versionbyte_length = 1   
mac_key = HMAC-SHA-256(CEK, "Microsoft SQL Server cell MAC key" + algorithm + CEK_length)  
```  
  
### <a name="step-4-concatenation"></a>Étape 4 : concaténation  
 Enfin, la valeur chiffrée est générée en concaténant simplement l'octet de version de l’algorithme, la valeur MAC, le vecteur d'initialisation et le texte chiffré AES_256_CBC :  
  
```  
aead_aes_256_cbc_hmac_sha_256 = versionbyte + MAC + IV + aes_256_cbc_ciphertext  
```  
  
## <a name="ciphertext-length"></a>Longueur du texte chiffré  
 Les longueurs (en octets) des composants particuliers du texte chiffré **AEAD_AES_256_CBC_HMAC_SHA_256** sont les suivantes :  
  
-   versionbyte: 1  
  
-   MAC: 32  
  
-   IV: 16  
  
-   aes_256_cbc_ciphertext: `(FLOOR (DATALENGTH(cell_data)/ block_size) + 1)* block_size`, où :  
  
    -   block_size est de 16 octets  
  
    -   cell_data est une valeur de texte en clair  
  
     Par conséquent, la taille minimale de aes_256_cbc_ciphertext est de 1 bloc, soit 16 octets.  
  
 Ainsi, la longueur du texte chiffré, qui résulte du chiffrement de valeurs en texte clair données (cell_data) peut être calculée à l'aide de la formule suivante :  
  
```  
1 + 32 + 16 + (FLOOR(DATALENGTH(cell_data)/16) + 1) * 16  
```  
  
 Exemple :  
  
-   Une valeur en texte clair **int** d’une longueur de 4 octets devient une valeur binaire d’une longueur de 65 octets après chiffrement.  
  
-   Une valeur en texte clair **nchar(1000)** d’une longueur de 2 000 octets devient une valeur binaire d’une longueur de 2 065 octets après chiffrement.  
  
 Le tableau suivant contient une liste complète des types de données et la longueur du texte chiffré pour chaque type.  
  
|Type de données|Longueur du texte chiffré [octets]|  
|---------------|---------------------------------|  
|**bigint**|65|  
|**binaire**|Variable. Utilisez la formule ci-dessus.|  
|**bit**|65|  
|**char**|Variable. Utilisez la formule ci-dessus.|  
|**date**|65|  
|**datetime**|65|  
|**datetime2**|65|  
|**datetimeoffset**|65|  
|**decimal**|81|  
|**float**|65|  
|**geography**|N/A (non pris en charge)|  
|**geometry**|N/A (non pris en charge)|  
|**hierarchyid**|N/A (non pris en charge)|  
|**image**|N/A (non pris en charge)|  
|**Int**|65|  
|**money**|65|  
|**nchar**|Variable. Utilisez la formule ci-dessus.|  
|**ntext**|N/A (non pris en charge)|  
|**numeric**|81|  
|**nvarchar**|Variable. Utilisez la formule ci-dessus.|  
|**real**|65|  
|**smalldatetime**|65|  
|**smallint**|65|  
|**smallmoney**|65|  
|**sql_variant**|N/A (non pris en charge)|  
|**sysname**|N/A (non pris en charge)|  
|**texte**|N/A (non pris en charge)|  
|**time**|65|  
|**timestamp**<br /><br /> (**rowversion**)|N/A (non pris en charge)|  
|**tinyint**|65|  
|**uniqueidentifier**|81|  
|**varbinary**|Variable. Utilisez la formule ci-dessus.|  
|**varchar**|Variable. Utilisez la formule ci-dessus.|  
|**xml**|N/A (non pris en charge)|  
  
## <a name="net-reference"></a>Référence .NET  
 Pour plus d’informations sur les algorithmes présentés dans ce document, consultez les fichiers **SqlAeadAes256CbcHmac256Algorithm.cs** et **SqlColumnEncryptionCertificateStoreProvider.cs** dans la [référence .NET](http://referencesource.microsoft.com/).  
  
## <a name="see-also"></a> Voir aussi  
 [Always Encrypted &#40;moteur de base de données&#41;](../../../relational-databases/security/encryption/always-encrypted-database-engine.md)   
 [Always Encrypted &#40;développement client&#41;](../../../relational-databases/security/encryption/always-encrypted-client-development.md)  
  
  
