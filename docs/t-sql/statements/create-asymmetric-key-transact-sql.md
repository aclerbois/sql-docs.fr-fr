---
title: CREATE ASYMMETRIC KEY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/07/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: t-sql|statements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- ASYMMETRIC_KEY_TSQL
- CREATE ASYMMETRIC KEY
- CREATE_ASYMMETRIC_KEY_TSQL
- ASYMMETRIC KEY
dev_langs:
- TSQL
helpviewer_keywords:
- asymmetric keys [SQL Server], creating
- encryption [SQL Server], asymmetric keys
- CREATE ASYMMETRIC KEY statement
- asymmetric keys [SQL Server]
- cryptography [SQL Server], asymmetric keys
ms.assetid: 141bc976-7631-49f6-82bd-a235028645b1
caps.latest.revision: 51
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 6ee7492ffa3c18730ac1bb10ce11ea11b0bfbc7d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-asymmetric-key-transact-sql"></a>CREATE ASYMMETRIC KEY (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Permet de créer une clé asymétrique dans la base de données.  
  
 Cette fonctionnalité est incompatible avec l'exportation de base de données à l'aide de l'infrastructure d'application de la couche Données. Vous devez supprimer toutes les clés asymétriques avant l'exportation.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
CREATE ASYMMETRIC KEY Asym_Key_Name   
   [ AUTHORIZATION database_principal_name ]  
   [ FROM <Asym_Key_Source> ]  
   [ WITH <key_option> ] 
   [ ENCRYPTION BY <encrypting_mechanism> ] 
   [ ; ]
  
<Asym_Key_Source>::=  
     FILE = 'path_to_strong-name_file'  
   | EXECUTABLE FILE = 'path_to_executable_file'  
   | ASSEMBLY Assembly_Name  
   | PROVIDER Provider_Name  
  
<key_option> ::=  
   ALGORITHM = <algorithm>  
      |  
   PROVIDER_KEY_NAME = 'key_name_in_provider'  
      |  
      CREATION_DISPOSITION = { CREATE_NEW | OPEN_EXISTING }  
  
<algorithm> ::=  
      { RSA_4096 | RSA_3072 | RSA_2048 | RSA_1024 | RSA_512 }   
  
<encrypting_mechanism> ::=  
    PASSWORD = 'password'   
```  
  
## <a name="arguments"></a>Arguments  
 FROM *Asym_Key_Source*  
 Spécifie la source à partir de laquelle la paire de clés asymétriques doit être chargée.  
  
 AUTHORIZATION *database_principal_name*  
 Spécifie le propriétaire de la clé asymétrique. Le propriétaire ne peut pas être un rôle ni un groupe. Si cette option n'est pas spécifiée, le propriétaire sera l'utilisateur en cours.  
  
 FILE ='*path_to_strong-name_file*'  
 Spécifie le chemin d'accès du fichier de nom à partir duquel il convient de charger la paire de clés.  
  
> [!NOTE]  
>  Cette option n'est pas disponible dans une base de données à relation contenant-contenu.  
  
 EXECUTABLE FILE ='*path_to_executable_file*'  
 Spécifie un fichier d'assembly à partir duquel il convient de charger la clé publique. Limité à 260 caractères par MAX_PATH dans l'API Windows.  
  
> [!NOTE]  
>  Cette option n'est pas disponible dans une base de données à relation contenant-contenu.  
  
 ASSEMBLY *Assembly_Name*  
 Spécifie le nom d'un assembly à partir duquel il convient de charger la clé publique.  
  
ENCRYPTION BY *\<key_name_in_provider>* Spécifie la façon dont la clé est chiffrée. Un certificat, un mot de passe ou une clé asymétrique peut être utilisé.  
  
 KEY_NAME ='*key_name_in_provider*'  
 Spécifie le nom de la clé à partir du fournisseur externe. Pour plus d’informations sur la gestion des clés externes, consultez [Gestion de clés extensible &#40;EKM&#41;](../../relational-databases/security/encryption/extensible-key-management-ekm.md).  
  
 CREATION_DISPOSITION = CREATE_NEW  
 Crée une clé sur le périphérique EKM (Extensible Key Management). La clause PROV_KEY_NAME doit être utilisée pour spécifier le nom de la clé sur le périphérique. Si une clé existe déjà sur le périphérique l'instruction échoue et génère une erreur.  
  
 CREATION_DISPOSITION = OPEN_EXISTING  
 Mappe une clé asymétrique [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à une clé EKM existante. La clause PROV_KEY_NAME doit être utilisée pour spécifier le nom de la clé sur le périphérique. Si CREATION_DISPOSITION = OPEN_EXISTING n'est pas spécifié, la valeur par défaut est CREATE_NEW.  
  
 ALGORITHM = \<algorithm>  
 Cinq algorithmes peuvent être fournis : RSA_4096, RSA_3072, RSA_2048, RSA_1024 et RSA_512.  
  
 RSA_1024 et RSA_512 sont dépréciés. Pour utiliser RSA_1024 ou RSA_512 (ce qui est déconseillé), vous devez affecter le niveau de compatibilité 120 ou un niveau inférieur à la base de données.  
  
 PASSWORD = '*password*'  
 Spécifie le mot de passe utilisé pour chiffrer la clé privée. Si cette clause n'est pas présente, la clé privée sera chiffrée à l'aide de la clé principale de la base de données. *password* comporte au maximum 128 caractères. *password* doit satisfaire aux critères de la stratégie de mot de passe Windows de l’ordinateur qui exécute l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="remarks"></a>Notes   
 Une *clé asymétrique* est une entité sécurisable au niveau base de données. Dans sa forme par défaut, cette entité contient à la fois une clé publique et une clé privée. Lorsqu'elle est exécutée sans la clause FROM, l'instruction CREATE ASYMMETRIC KEY génère une nouvelle paire de clés. Lorsqu'elle est exécutée avec la clause FROM, l'instruction CREATE ASYMMETRIC KEY importe une paire de clés à partir d'un fichier ou importe une clé publique à partir d'un assembly.  
  
 Par défaut, la clé privée est protégée par la clé principale de base de données. Si aucune clé principale de base de données n'a été créée, un mot de passe est requis pour protéger la clé privée. Si une clé principale de base de données existe, le mot de passe est facultatif.  
  
 La clé privée peut compter 512, 1 024 ou 2 048 bits.  
  
## <a name="permissions"></a>Autorisations  
 Requiert l'autorisation CREATE ASYMMETRIC KEY sur la base de données. Si la clause AUTHORIZATION est spécifiée, l'autorisation IMPERSONATE sur le principal de base de données ou l'autorisation ALTER sur le rôle d'application est requise. Les connexions Windows, les connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et les rôles d’application sont les seuls à pouvoir posséder des clés asymétriques. Les groupes et les rôles ne peuvent pas posséder de clés asymétriques.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-creating-an-asymmetric-key"></a>A. Création d'une clé asymétrique  
 Dans l'exemple suivant, une clé asymétrique nommée `PacificSales09` est créée à l'aide de l'algorithme `RSA_2048` et la clé privée est protégée par un mot de passe.  
  
```  
CREATE ASYMMETRIC KEY PacificSales09   
    WITH ALGORITHM = RSA_2048   
    ENCRYPTION BY PASSWORD = '<enterStrongPasswordHere>';   
GO  
```  
  
### <a name="b-creating-an-asymmetric-key-from-a-file-giving-authorization-to-a-user"></a>B. Création d'une clé asymétrique à partir d'un fichier, octroi d'une autorisation à un utilisateur  
 Dans l'exemple ci-dessous, la clé asymétrique `PacificSales19` est créée à partir d'une paire de clés stockées dans un fichier, puis l'utilisateur `Christina` est autorisé à utiliser la clé asymétrique.  
  
```  
CREATE ASYMMETRIC KEY PacificSales19 AUTHORIZATION Christina   
    FROM FILE = 'c:\PacSales\Managers\ChristinaCerts.tmp'    
    ENCRYPTION BY PASSWORD = '<enterStrongPasswordHere>';  
GO  
```  
  
### <a name="c-creating-an-asymmetric-key-from-an-ekm-provider"></a>C. Création d'une clé asymétrique à partir d'un fournisseur EKM  
 L'exemple suivant crée le clé asymétrique `EKM_askey1` à partir d'une paire de clés stockées dans un fichier. La clé est ensuite chiffrée à l'aide d'un fournisseur EKM, appelé `EKMProvider1`, et d'une clé sur ce fournisseur appelée `key10_user1`.  
  
```  
CREATE ASYMMETRIC KEY EKM_askey1   
    FROM PROVIDER EKM_Provider1  
    WITH   
        ALGORITHM = RSA_2048,   
        CREATION_DISPOSITION = CREATE_NEW  
        , PROVIDER_KEY_NAME  = 'key10_user1' ;  
GO  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Choisir un algorithme de chiffrement](../../relational-databases/security/encryption/choose-an-encryption-algorithm.md)   
 [ALTER ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-asymmetric-key-transact-sql.md)   
 [DROP ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-asymmetric-key-transact-sql.md)   
 [Hiérarchie de chiffrement](../../relational-databases/security/encryption/encryption-hierarchy.md)   
 [Gestion de clés extensible à l’aide d’Azure Key Vault &#40;SQL Server&#41;](../../relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server.md)  
  
  
