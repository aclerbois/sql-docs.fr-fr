---
title: CREATE CERTIFICATE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: t-sql|statements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- CERTIFICATE
- CREATE_CERTIFICATE_TSQL
- sql13.swb.createcertificate.f1
- CERTIFICATE_TSQL
- CREATE CERTIFICATE
- sql13.swb.certificateproperties.f1
dev_langs:
- TSQL
helpviewer_keywords:
- encryption [SQL Server], certificates
- certificates [SQL Server], adding
- certificates [SQL Server]
- adding certificates
- cryptography [SQL Server], certificates
- CREATE CERTIFICATE statement
ms.assetid: a4274b2b-4cb0-446a-a956-1c8e6587515d
caps.latest.revision: 74
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: abd46332127b1b15280df0ec5bab5655da5a148a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-certificate-transact-sql"></a>CREATE CERTIFICATE (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  Permet d'ajouter un certificat à une base de données dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  

[!INCLUDE[ssMIlimitation](../../includes/sql-db-mi-limitation.md)]

 Cette fonctionnalité est incompatible avec l'exportation de base de données à l'aide de l'infrastructure d'application de la couche Données. Vous devez supprimer tous les certificats avant l'exportation.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
-- Syntax for SQL Server and Azure SQL Database  
  
CREATE CERTIFICATE certificate_name [ AUTHORIZATION user_name ]   
    { FROM <existing_keys> | <generate_new_keys> }  
    [ ACTIVE FOR BEGIN_DIALOG =  { ON | OFF } ]  
  
<existing_keys> ::=   
    ASSEMBLY assembly_name  
    | {   
        [ EXECUTABLE ] FILE = 'path_to_file'  
        [ WITH PRIVATE KEY ( <private_key_options> ) ]   
      }  
    | {   
        BINARY = asn_encoded_certificate  
        [ WITH PRIVATE KEY ( <private_key_options> ) ]  
      }  
<generate_new_keys> ::=   
    [ ENCRYPTION BY PASSWORD = 'password' ]   
    WITH SUBJECT = 'certificate_subject_name'   
    [ , <date_options> [ ,...n ] ]   
  
<private_key_options> ::=  
      {   
        FILE = 'path_to_private_key'  
         [ , DECRYPTION BY PASSWORD = 'password' ]  
         [ , ENCRYPTION BY PASSWORD = 'password' ]    
      }  
    |  
      {   
        BINARY = private_key_bits  
         [ , DECRYPTION BY PASSWORD = 'password' ]  
         [ , ENCRYPTION BY PASSWORD = 'password' ]    
      }  
  
<date_options> ::=  
    START_DATE = 'datetime' | EXPIRY_DATE = 'datetime'  
```  
  
```  
-- Syntax for Azure SQL Data Warehouse and Parallel Data Warehouse  
  
CREATE CERTIFICATE certificate_name   
    { <generate_new_keys> | FROM <existing_keys> }  
    [ ; ]  
  
<generate_new_keys> ::=   
    WITH SUBJECT ='certificate_subject_name'   
    [ , <date_options> [ ,...n ] ]   
  
<existing_keys> ::=   
    {   
      FILE ='path_to_file'  
      WITH PRIVATE KEY   
         (   
           FILE ='path_to_private_key'  
           , DECRYPTION BY PASSWORD ='password'   
         )  
    }  
  
<date_options> ::=  
    START_DATE ='datetime' | EXPIRY_DATE ='datetime'  
```  
  
## <a name="arguments"></a>Arguments  
 *certificate_name*  
 Nom du certificat dans la base de données.  
  
 AUTHORIZATION *user_name*  
 Nom de l’utilisateur auquel appartient ce certificat.  
  
 ASSEMBLY *assembly_name*  
 Spécifie un assembly signé qui a déjà été chargé dans la base de données.  
  
 [ EXECUTABLE ] FILE ='*path_to_file*'  
 Spécifie le chemin d'accès complet, y compris le nom de fichier, d'un fichier encodé DER qui contient le certificat. Si l'option EXECUTABLE est utilisée, le fichier est une DLL signée par le certificat. *path_to_file* peut être un chemin local ou un chemin UNC d’un emplacement réseau. L’accès au fichier s’effectue dans le contexte de sécurité du compte de service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Ce compte doit posséder les autorisations de système de fichiers requises.  
  
 WITH PRIVATE KEY  
 Spécifie que la clé privée du certificat est chargée dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cette clause est valide uniquement lorsque le certificat est créé à partir d'un fichier. Pour charger la clé privée d’un assembly, utilisez [ALTER CERTIFICATE](../../t-sql/statements/alter-certificate-transact-sql.md).  
  
 FILE ='*path_to_private_key*'  
 Spécifie le chemin d'accès complet, y compris le nom du fichier, à la clé privée. *path_to_private_key* peut être un chemin local ou un chemin UNC d’un emplacement réseau. L’accès au fichier s’effectue dans le contexte de sécurité du compte de service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Ce compte doit posséder les autorisations de système de fichiers nécessaires.  
  
> [!NOTE]  
>  Cette option n'est pas disponible dans une base de données à relation contenant-contenu.  
  
 asn_encoded_certificate  
 Bits de certificat encodés par ASN spécifiés comme constante binaire.  
  
 BINARY =*private_key_bits*  
 **S'applique à**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] jusqu'à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)].  
  
 Bits de clé privée spécifiés comme constante binaire. Ces bits peuvent être sous forme chiffrée. Si chiffrés, l'utilisateur doit fournir un mot de passe de déchiffrement. Les contrôles de stratégie de mot de passe ne sont pas exécutés sur ce mot de passe. Les bits de clé privée doivent être dans un format de fichier PVK.  
  
 DECRYPTION BY PASSWORD ='*key_password*'  
 Spécifie le mot de passe requis pour déchiffrer une clé privée récupérée à partir d'un fichier. Cette clause est facultative si la clé privée est protégée par un mot de passe vide. Il n'est pas conseillé d'enregistrer une clé privée dans un fichier sans la protéger par un mot de passe. Si un mot de passe est nécessaire mais qu’aucun mot de passe n’est spécifié, l’instruction échoue.  
  
 ENCRYPTION BY PASSWORD ='*password*'  
 Spécifie le mot de passe utilisé pour chiffrer la clé privée. Utilisez cette option seulement si vous voulez chiffrer le certificat à l'aide d'un mot de passe. Si cette clause est omise, la clé privée est chiffrée à l’aide de la clé principale de base de données. *password* doit satisfaire aux critères de la stratégie de mot de passe Windows de l’ordinateur qui exécute l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d'informations, consultez [Password Policy](../../relational-databases/security/password-policy.md).  
  
 SUBJECT ='*certificate_subject_name*'  
 Le terme *subject* fait référence à un champ dans les métadonnées du certificat, tel que défini dans la norme X.509. L’objet ne doit pas faire plus de 64 caractères, et cette limite est appliquée pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur Linux. Pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur Windows, l’objet peut contenir jusqu’à 128 caractères. Les objets de plus de 128 caractères sont tronqués quand ils sont stockés dans le catalogue, mais l’objet blob (binary large object) qui contient le certificat conserve le nom complet de l’objet.  
  
 START_DATE ='*datetime*'  
 Date à laquelle le certificat devient valide. Si cette date n’est pas spécifiée, la date actuelle est attribuée à START_DATE. START_DATE utilise l'heure UTC et peut être spécifié selon tout format convertible en date et heure.  
  
 EXPIRY_DATE ='*datetime*'  
 Date à laquelle le certificat expire. Si cette date n’est pas spécifiée, EXPIRY_DATE a une valeur égale à un an après START_DATE. EXPIRY_DATE utilise l'heure UTC et peut être spécifié selon tout format convertible en date et heure. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Service Broker vérifie la date d’expiration. Toutefois, l’expiration n’est pas appliquée quand le certificat est utilisé pour le chiffrement.  
  
 ACTIVE FOR BEGIN_DIALOG = { **ON** | OFF }  
 Met le certificat à disposition de l'initiateur d'une conversation [!INCLUDE[ssSB](../../includes/sssb-md.md)]. La valeur par défaut est ON.  
  
## <a name="remarks"></a>Notes   
 Un certificat est un élément sécurisable de niveau base de données qui suit la norme X.509 et prend en charge les champs X.509 V1. CREATE CERTIFICATE permet de charger un certificat à partir d'un fichier ou d'un assembly. Cette instruction peut également générer une paire de clés et créer un certificat autosigné.  
  
 La clé privée doit être \<= 2500 octets dans un format chiffré. Les clés privées générées par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] font 1024 bits jusqu’à [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], et 2 048 bits à compter de [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)]. Les clés privées importées à partir d'une source externe ont une longueur minimale de 384 bits et une longueur maximale de 4 096 bits. La longueur d'une clé privée importée doit être un entier multiple de 64 bits. Les certificats utilisés pour le chiffrement transparent des données sont limités à une taille de clé privée de 3456 bits.  
  
 Le numéro de série entier du certificat est stocké, mais seuls les 16 premiers octets apparaissent dans la vue de catalogue sys.certificates.  
  
 Le champ entier de l’émetteur du certificat est stocké, mais seuls les 884 premiers octets apparaissent dans la vue de catalogue sys.certificates.  
  
 La clé privée doit correspondre à la clé publique spécifiée par *certificate_name*.  
  
 Lorsque vous créez un certificat à partir d'un conteneur, le chargement de la clé privée est facultatif. En revanche, lorsque [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] génère un certificat autosigné, la clé privée est toujours créée. Par défaut, la clé privée est chiffrée au moyen de la clé principale de base de données. Si la clé principale de base de données n’existe pas et qu’aucun mot de passe n’est spécifié, l’instruction échoue.  
  
 L’option ENCRYPTION BY PASSWORD n’est pas nécessaire quand la clé privée est chiffrée au moyen de la clé principale de base de données. Utilisez cette option seulement quand la clé privée est chiffrée à l’aide d’un mot de passe. Si aucun mot de passe n'est spécifié, la clé privée du certificat sera chiffrée à l'aide de la clé principale de base de données. Si la clé principale de la base de données ne peut pas être ouverte, ne pas spécifier cette clause entraîne une erreur.  
  
 Vous n'êtes pas obligé de spécifier un mot de passe de déchiffrement lorsque la clé privée est chiffrée au moyen de la clé principale de base de données.  
  
> [!NOTE]  
>  Les fonctions intégrées de chiffrement et de signature ne vérifient pas les dates d'expiration des certificats. Les utilisateurs de ces fonctions doivent décider quand vérifier l'expiration des certificats.  
  
 Vous pouvez créer une description binaire d’un certificat à l’aide des fonctions [CERTENCODED &#40;Transact-SQL&#41;](../../t-sql/functions/certencoded-transact-sql.md) et [CERTPRIVATEKEY &#40;Transact-SQL&#41;](../../t-sql/functions/certprivatekey-transact-sql.md). Pour obtenir un exemple qui utilise **CERTPRIVATEKEY** et **CERTENCODED** afin de copier un certificat dans une autre base de données, consultez l’exemple B de la rubrique [CERTENCODED &#40;Transact-SQL&#41;](../../t-sql/functions/certencoded-transact-sql.md).  
  
## <a name="permissions"></a>Autorisations  
 Requiert l'autorisation CREATE CERTIFICATE sur la base de données. Les connexions Windows, les connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et les rôles d’application sont les seuls à pouvoir posséder des certificats. Les groupes et les rôles ne peuvent pas posséder de certificats.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-creating-a-self-signed-certificate"></a>A. Création d'un certificat auto-signé  
 L'exemple suivant crée un certificat nommé `Shipping04`. La clé privée de ce certificat est protégée à l'aide d'un mot de passe.  
  
```  
CREATE CERTIFICATE Shipping04   
   ENCRYPTION BY PASSWORD = 'pGFD4bb925DGvbd2439587y'  
   WITH SUBJECT = 'Sammamish Shipping Records',   
   EXPIRY_DATE = '20201031';  
GO  
```  
  
### <a name="b-creating-a-certificate-from-a-file"></a>B. Création d'un certificat à partir d'un fichier  
 L'exemple ci-dessous crée un certificat dans la base de données en chargeant la paire de clés à partir de fichiers.  
  
```  
CREATE CERTIFICATE Shipping11   
    FROM FILE = 'c:\Shipping\Certs\Shipping11.cer'   
    WITH PRIVATE KEY (FILE = 'c:\Shipping\Certs\Shipping11.pvk',   
    DECRYPTION BY PASSWORD = 'sldkflk34et6gs%53#v00');  
GO   
```  
  
### <a name="c-creating-a-certificate-from-a-signed-executable-file"></a>C. Création d'un certificat à partir d'un fichier exécutable signé  
  
```  
CREATE CERTIFICATE Shipping19   
    FROM EXECUTABLE FILE = 'c:\Shipping\Certs\Shipping19.dll';  
GO  
```  
  
 Une autre méthode consiste à créer un assembly à partir du fichier `dll`, puis à créer un certificat à partir de l'assembly.  
  
```  
CREATE ASSEMBLY Shipping19   
    FROM ' c:\Shipping\Certs\Shipping19.dll'   
    WITH PERMISSION_SET = SAFE;  
GO  
CREATE CERTIFICATE Shipping19 FROM ASSEMBLY Shipping19;  
GO  
```  
  
### <a name="d-creating-a-self-signed-certificate"></a>D. Création d'un certificat auto-signé  
 L’exemple suivant crée un certificat nommé `Shipping04` sans spécifier de mot de passe de chiffrement. Cet exemple peut être utilisé avec [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)].
  
```  
CREATE CERTIFICATE Shipping04   
   WITH SUBJECT = 'Sammamish Shipping Records';  
GO  
```  
  
  
## <a name="see-also"></a> Voir aussi  
 [ALTER CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-certificate-transact-sql.md)   
 [DROP CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-certificate-transact-sql.md)   
 [BACKUP CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/backup-certificate-transact-sql.md)   
 [Hiérarchie de chiffrement](../../relational-databases/security/encryption/encryption-hierarchy.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)   
 [CERTENCODED &#40;Transact-SQL&#41;](../../t-sql/functions/certencoded-transact-sql.md)   
 [CERTPRIVATEKEY &#40;Transact-SQL&#41;](../../t-sql/functions/certprivatekey-transact-sql.md)  
  
  


