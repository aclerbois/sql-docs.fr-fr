---
title: sp_refresh_parameter_encryption (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 01/11/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sp_refresh_parameter_encryption
- sp_refresh_parameter_encryption_TSQL
- sys.sp_refresh_parameter_encryption
- sys.sp_refresh_parameter_encryption_TSQL
helpviewer_keywords:
- sp_refresh_parameter_encryption
- Always Encrypted, sp_refresh_parameter_encryption
ms.assetid: 00b44baf-fcf0-4095-aabe-49fa87e77316
caps.latest.revision: 3
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 1af1ab9933bab98ab0679749d47f4c6ec4a86bc2
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sprefreshparameterencryption-transact-sql"></a>sp_refresh_parameter_encryption (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2016-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2016-asdb-xxxx-xxx-md.md)]

Met à jour les métadonnées de chiffrement intégral pour les paramètres de la procédure non liée au schéma spécifié, fonction définie par l’utilisateur, vue, DML déclencheur, déclencheur DDL au niveau de la base de données ou déclencheur DDL de niveau serveur dans la base de données actuelle. 

 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
sys.sp_refresh_parameter_encryption [ @name = ] 'module_name' 
    [ , [ @namespace = ] '<class>' ]
[ ; ]

<class> ::=
{ DATABASE_DDL_TRIGGER | SERVER_DDL_TRIGGER }
```

## <a name="arguments"></a>Arguments

[  **@name =** ] **'***nom_module***'**   
Nom de la procédure stockée, fonction définie par l'utilisateur, vue, déclencheur DML, déclencheur DDL au niveau de la base de données ou déclencheur DDL au niveau du serveur. *nom_module* ne peut pas être un common language runtime (CLR) procédure stockée ou une fonction CLR. *nom_module* ne peut pas être liée au schéma. *nom_module* est `nvarchar`, sans valeur par défaut. *nom_module* peut être un identificateur multipartie, mais ne peut faire référence à des objets dans la base de données actuelle.

[  **@namespace =** ] **'** < classe > **'**   
Classe du module spécifié. Lorsque *nom_module* est un déclencheur DDL, `<class>` est requis. `<class>` a la valeur `nvarchar(20)`. Les entrées valides sont `DATABASE_DDL_TRIGGER` et `SERVER_DDL_TRIGGER`.    

## <a name="return-code-values"></a>Valeurs des codes de retour  

0 (réussite) ou un nombre différent de zéro (échec)


## <a name="remarks"></a>Notes

Les métadonnées de chiffrement pour les paramètres d’un module peuvent devenir obsolète, si :   
* Propriétés de chiffrement d’une colonne dans une table de références de module, ont été mis à jour. Par exemple, une colonne a été supprimée et une nouvelle colonne portant le même nom, mais un type de chiffrement, clé de chiffrement ou un algorithme de chiffrement a été ajoutée.  
* Le module fait référence à un autre module avec les métadonnées de chiffrement de paramètre obsolète.  

Lorsque les propriétés de chiffrement d’une table sont modifiées, `sp_refresh_parameter_encryption` doit être exécuté pour tous les modules directement ou indirectement référence à la table. Cette procédure stockée peut être appelée sur ces modules dans n’importe quel ordre, sans nécessiter de l’utilisateur à la première actualisation le module interne avant de passer à ses appelants.

`sp_refresh_parameter_encryption` n’affecte pas les autorisations, propriétés étendues, ou `SET` options qui sont associées à l’objet. 

Pour actualiser un déclencheur DDL au niveau du serveur, exécutez cette procédure stockée à partir du contexte de toute base de données.

>  [!NOTE]   
>  Les signatures qui sont associés à l’objet sont supprimées lorsque vous exécutez `sp_refresh_parameter_encryption`.

## <a name="permissions"></a>Autorisations

Requiert `ALTER` l’autorisation sur le module et `REFERENCES` autorisation sur les types CLR définis par l’utilisateur et les collections de schémas XML qui sont référencées par l’objet.   

Lorsque le module spécifié est un déclencheur DDL de niveau base de données, nécessite `ALTER ANY DATABASE DDL TRIGGER` autorisation dans la base de données actuelle.    

Lorsque le module spécifié est un déclencheur DDL de niveau serveur, nécessite `CONTROL SERVER` autorisation.

Pour les modules qui sont définies avec la `EXECUTE AS` clause, `IMPERSONATE` autorisation est requise sur le principal spécifié. En règle générale, l’actualisation d’un objet ne modifie pas son `EXECUTE AS` principal, sauf si le module a été défini avec `EXECUTE AS USER` et le nom d’utilisateur du principal maintenant résout à un autre utilisateur qu’au moment où le module a été créé.
 
## <a name="examples"></a>Exemples

L’exemple suivant crée une table et une procédure qui référencent la table, configure Always Encrypted, puis illustre la modification de la table et en cours d’exécution le `sp_refresh_parameter_encryption` procédure.  

Tout d’abord créer la table initiale et une procédure stockée faisant référence à la table.
```sql
CREATE TABLE [Patients]([PatientID] [int] IDENTITY(1,1) NOT NULL,
    [SSN] [char](11), 
    [FirstName] [nvarchar](50) NULL,
    [LastName] [nvarchar](50) NOT NULL,
    [MiddleName] [nvarchar](50) NULL,
    [StreetAddress] [nvarchar](50) NOT NULL,
    [City] [nvarchar](50) NOT NULL,
    [ZipCode] [char](5) NOT NULL,
    [State] [char](2) NOT NULL,
    [BirthDate] [date] NOT NULL,
 CONSTRAINT [PK_Patients] PRIMARY KEY CLUSTERED 
(
    [PatientID] ASC
) WITH 
    (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, 
     IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, 
     ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]) ON [PRIMARY];
GO

CREATE PROCEDURE [find_patient] @SSN [char](11)
AS
BEGIN
    SELECT * FROM [Patients] WHERE SSN=@SSN
END;
GO
```

Définissez ensuite les clés Always Encrypted.
```sql
CREATE COLUMN MASTER KEY [CMK1]
WITH
(
       KEY_STORE_PROVIDER_NAME = N'MSSQL_CERTIFICATE_STORE',
    KEY_PATH = N'CurrentUser/my/A66BB0F6DD70BDFF02B62D0F87E340288E6F9305'
);
GO

CREATE COLUMN ENCRYPTION KEY [CEK1]
WITH VALUES
(
       COLUMN_MASTER_KEY = [CMK1],
    ALGORITHM = 'RSA_OAEP',
    ENCRYPTED_VALUE = 
       0x016E000001630075007200720065006E00740075007300650072002F006D0079002F006100360036006200620030006600360064006400370030006200640066006600300032006200360032006400300066003800370065003300340030003200380038006500360066003900330030003500CA0D0CEC74ECADD1804CF99137B4BD06BBAB15D7EA74E0C249A779C7768A5B659E0125D24FF827F5EA8CA517A8E197ECA1353BA814C2B0B2E6C8AB36E3AE6A1E972D69C3C573A963ADAB6686CF5D24F95FE43140C4F9AF48FBA7DF2D053F3B4A1F5693A1F905440F8015BDB43AF8A04BE4E045B89876A0097E5FBC4E6A3B9C3C0D278C540E46C53938B8C957B689C4DC095821C465C73117CBA95B758232F9E5B2FCC7950B8CA00AFE374DE42847E3FBC2FDD277035A2DEF529F4B735C20D980073B4965B4542A34723276A1646998FC6E1C40A3FDB6ABCA98EE2B447F114D2AC7FF8C7D51657550EC5C2BABFFE8429B851272086DCED94332CF18FA854C1D545A28B1EF4BE64F8E035175C1650F6FC5C4702ACF99850A4542B3747EAEC0CC726E091B36CE24392D801ECAA684DE344FECE05812D12CD72254A014D42D0EABDA41C89FC4F545E88B4B8781E5FAF40D7199D4842D2BFE904D209728ED4F527CBC169E2904F6E711FF81A8F4C25382A2E778DD2A58552ED031AFFDA9D9D891D98AD82155F93C58202FC24A77F415D4F8EF22419D62E188AC609330CCBD97CEE1AEF8A18B01958833604707FDF03B2B386487CC679D7E352D0B69F9FB002E51BCD814D077E82A09C14E9892C1F8E0C559CFD5FA841CEF647DAB03C8191DC46B772E94D579D8C80FE93C3827C9F0AE04D5325BC73111E07EEEDBE67F1E2A73580085
);
GO
```


Enfin, nous remplaçons la colonne SSN avec la colonne chiffrée, puis exécute le `sp_refresh_parameter_encryption` procédure pour mettre à jour les composants Always Encrypted.
```sql
ALTER TABLE [Patients] DROP COLUMN [SSN];
GO

ALTER TABLE [Patients] 
    ADD [SSN] [char](11) COLLATE Latin1_General_BIN2 
    ENCRYPTED WITH 
        (COLUMN_ENCRYPTION_KEY = [CEK1], 
        ENCRYPTION_TYPE = Deterministic, 
        ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') 
    NOT NULL;
GO

EXEC sp_refresh_parameter_encryption [find_patient];
GO
```

## <a name="see-also"></a>Voir aussi 

[Always Encrypted](../../relational-databases/security/encryption/always-encrypted-database-engine.md)   
[Assistant Always Encrypted](../../relational-databases/security/encryption/always-encrypted-wizard.md)   

