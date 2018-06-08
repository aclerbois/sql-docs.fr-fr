---
title: CERTPROPERTY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: t-sql|functions
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- CERTPROPERTY
- CERTPROPERTY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- certificates [SQL Server], schema names
- schemas [SQL Server], names
- CERTPROPERTY function
ms.assetid: 966c09aa-bc4e-45b0-ba53-c8381871f638
caps.latest.revision: 22
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 986553e34dd8f4093ffe3ed7a54e979e048418e9
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="certproperty-transact-sql"></a>CERTPROPERTY (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

Retourne la valeur d'une propriété de certificat spécifiée.
  
![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntaxe  
  
```sql
CertProperty ( Cert_ID , '<PropertyName>' )  
  
<PropertyName> ::=  
   Expiry_Date | Start_Date | Issuer_Name   
   | Cert_Serial_Number | Subject | SID | String_SID   
```  
  
## <a name="arguments"></a>Arguments  
*Cert_ID*  
Valeur d’ID de certificat, du type de données int.
  
*Expiry_Date*  
Date d'expiration du certificat.
  
*Start_Date*  
Date à laquelle le certificat devient valide.
  
*Issuer_Name*  
Nom de l’émetteur du certificat.
  
*Cert_Serial_Number*  
Numéro de série du certificat.
  
*Objet*  
Objet du certificat
  
 *SID*  
SID du certificat C'est également le SID de n'importe quelle connexion ou utilisateur mappés à ce certificat.
  
*String_SID*  
SID du certificat, sous forme de chaîne de caractères. C'est également le SID de n'importe quelle connexion ou utilisateur mappés à ce certificat.
  
## <a name="return-types"></a>Types de retour
La spécification de la propriété doit être placée dans des guillemets simples.
  
Le type de valeur retournée dépend de la propriété qui est spécifiée dans l’appel de fonction. Le type de retour **sql_variant** wrappe toutes les valeurs de retour.
-   *Expiry_Date* et *Start_Date* renvoient **datetime**.  
-   *Cert_Serial_Number*, *Issuer_Name*, *String_SID*, et *Subject* retournent **nvarchar**.  
-   *SID* renvoie **varbinary**.  
  
## <a name="remarks"></a>Notes   
Ouvrez la vue de catalogue [sys.certificates](../../relational-databases/system-catalog-views/sys-certificates-transact-sql.md) pour consulter les informations relatives aux certificats.
  
## <a name="permissions"></a>Autorisations  
Nécessite des autorisations sur le certificat, et nécessite que l’appelant ne se soit pas vu refuser l’autorisation VIEW pour le certificat. Pour plus d’informations sur les autorisations de certificat, consultez [CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/create-certificate-transact-sql.md) et [GRANT CERTIFICATE PERMISSIONS &#40;Transact-SQL&#41;](../../t-sql/statements/grant-certificate-permissions-transact-sql.md).
  
## <a name="examples"></a>Exemples  
L'exemple suivant retourne l'objet du certificat.
  
```sql
-- First create a certificate.  
CREATE CERTIFICATE Marketing19 WITH   
    START_DATE = '04/04/2004' ,  
    EXPIRY_DATE = '07/07/2040' ,  
    SUBJECT = 'Marketing Print Division';  
GO  
  
-- Now use CertProperty to examine certificate  
-- Marketing19's properties.  
DECLARE @CertSubject sql_variant;  
set @CertSubject = CertProperty( Cert_ID('Marketing19'), 'Subject');  
PRINT CONVERT(nvarchar, @CertSubject);  
GO  
```  
  
## <a name="see-also"></a>Voir aussi
[CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/create-certificate-transact-sql.md)  
[ALTER CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-certificate-transact-sql.md)  
[CERT_ID &#40;Transact-SQL&#41;](../../t-sql/functions/cert-id-transact-sql.md)
[Hiérarchie de chiffrement](../../relational-databases/security/encryption/encryption-hierarchy.md)
[sys.certificates &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-certificates-transact-sql.md) 
[Vues de catalogue de sécurité &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)
  
  
