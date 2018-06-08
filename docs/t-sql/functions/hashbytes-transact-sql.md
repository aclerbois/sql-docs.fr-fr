---
title: HASHBYTES (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/29/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: t-sql|functions
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- HASHBYTES_TSQL
- HASHBYTES
dev_langs:
- TSQL
helpviewer_keywords:
- hash input
- HASHBYTES
ms.assetid: 0ea6a4d1-313e-4f70-b939-dd2cd570f6d6
caps.latest.revision: 38
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 20e5e1a2f85d81a49072fba1e6acae3bc9b1dc1e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="hashbytes-transact-sql"></a>HASHBYTES (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  Retourne le hachage MD2, MD4, MD5, SHA, SHA1 ou SHA2 des données d'entrée dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
HASHBYTES ( '<algorithm>', { @input | 'input' } )  
  
<algorithm>::= MD2 | MD4 | MD5 | SHA | SHA1 | SHA2_256 | SHA2_512   
```  
  
## <a name="arguments"></a>Arguments  
 **'**\<algorithm>**'**  
 Identifie l'algorithme de hachage à utiliser pour les données d'entrée. Cet argument est obligatoire, sans valeur par défaut. Les guillemets simples sont obligatoires. À partir de [!INCLUDE[ssSQL15](../../includes/sssql15-md.md)], tous les algorithmes autres que SHA2_256 et SHA2_512 sont déconseillés. Des algorithmes plus anciens (non recommandés) continueront de fonctionner, mais ils déclencheront un événement de dépréciation.  
  
 **@input**  
 Variable contenant les données à hacher. **@input** est de type **varchar**, **nvarchar** ou **varbinary**.  
  
 **'** *input* **'**  
 Spécifie une expression qui correspond à une chaîne de type caractère ou binaire à hacher.  
  
 La sortie se conforme à l'algorithme standard : 128 bits (16 octets) pour MD2, MD4 et MD5 ; 160 bits (20 octets) pour SHA et SHA1 ; 256 bits (32 octets) pour SHA2_256 et 512 bits (64 octets) pour SHA2_512.  
  
**S’applique à**: [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] jusqu’à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]
  
 Pour [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et versions antérieures, les valeurs d’entrée autorisées sont limitées à 8 000 octets.  
  
## <a name="return-value"></a>Valeur retournée  
 **varbinary** (au maximum 8 000 octets)  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-return-the-hash-of-a-variable"></a>A : Renvoie le hachage d’une variable  
 L’exemple suivant renvoie le hachage `SHA1` des données **nvarchar** stockées dans la variable `@HashThis`.  
  
```  
DECLARE @HashThis nvarchar(4000);  
SET @HashThis = CONVERT(nvarchar(4000),'dslfdkjLK85kldhnv$n000#knf');  
SELECT HASHBYTES('SHA1', @HashThis);  
  
```  
  
### <a name="b-return-the-hash-of-a-table-column"></a>B : Renvoie le hachage d’une colonne de table  
 L'exemple suivant retourne le hachage SHA1 des valeurs de la colonne, `c1` dans la table `Test1`.  
  
```  
CREATE TABLE dbo.Test1 (c1 nvarchar(50));  
INSERT dbo.Test1 VALUES ('This is a test.');  
INSERT dbo.Test1 VALUES ('This is test 2.');  
SELECT HASHBYTES('SHA1', c1) FROM dbo.Test1;  
  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
  
-------------------------------------------  
0x0E7AAB0B4FF0FD2DFB4F0233E2EE7A26CD08F173  
0xF643A82F948DEFB922B12E50B950CEE130A934D6  
  
(2 row(s) affected)  
  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Choisir un algorithme de chiffrement](../../relational-databases/security/encryption/choose-an-encryption-algorithm.md)  
  
  
