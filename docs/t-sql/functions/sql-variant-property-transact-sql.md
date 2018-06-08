---
title: SQL_VARIANT_PROPERTY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/12/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: t-sql|functions
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- SQL_VARIANT_PROPERTY_TSQL
- SQL_VARIANT_PROPERTY
dev_langs:
- TSQL
helpviewer_keywords:
- SQL_VARIANT_PROPERTY function
- sql_variant data type
ms.assetid: 50e5c1d9-4e95-4ed0-9c92-435c872a399e
caps.latest.revision: 42
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 125a56a896f784ceb08bfd0c0ae2f10b03e802c8
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlvariantproperty-transact-sql"></a>SQL_VARIANT_PROPERTY (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  Retourne le type de données de base et d’autres informations sur une valeur **sql_variant**.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
SQL_VARIANT_PROPERTY ( expression , property )  
```  
  
## <a name="arguments"></a>Arguments  
 *expression*  
 Expression de type **sql_variant**.  
  
 *property*  
 Contient le nom de la propriété **sql_variant** dont les informations doivent être fournies. *property* est de type **varchar(** 128 **)** et peut prendre l’une des valeurs suivantes :  
  
|Valeur|Description|Type de base sql_variant renvoyé|  
|-----------|-----------------|----------------------------------------|  
|**BaseType**|Type de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], tel que :<br /><br /> **bigint**<br /><br /> **binaire**<br /><br /> **char**<br /><br /> **date**<br /><br /> **datetime**<br /><br /> **datetime2**<br /><br /> **datetimeoffset**<br /><br /> **decimal**<br /><br /> **float**<br /><br /> **Int**<br /><br /> **money**<br /><br /> **nchar**<br /><br /> **numeric**<br /><br /> **nvarchar**<br /><br /> **real**<br /><br /> **smalldatetime**<br /><br /> **smallint**<br /><br /> **smallmoney**<br /><br /> **time**<br /><br /> **tinyint**<br /><br /> **uniqueidentifier**<br /><br /> **varbinary**<br /><br /> **varchar**|**sysname**<br /><br /> NULL = Entrée non valide.|  
|**Précision**|Nombre de chiffres du type de données numériques de base :<br /><br /> **datetime** = 23<br /><br /> **smalldatetime** = 16<br /><br /> **float** = 53<br /><br /> **real** = 24<br /><br /> **decimal** (p,s) et **numeric** (p,s) = p<br /><br /> **money** = 19<br /><br /> **smallmoney** = 10<br /><br /> **bigint** = 19<br /><br /> **int** = 10<br /><br /> **smallint** = 5<br /><br /> **tinyint** = 3<br /><br /> **bit** = 1<br /><br /> Tous les autres types = 0|**Int**<br /><br /> NULL = Entrée non valide.|  
|**Échelle**|Nombre de chiffres décimaux après la virgule (point) dans le type de données numériques de base :<br /><br /> **decimal** (p,s) et **numeric** (p,s) = s<br /><br /> **money** et **smallmoney** = 4<br /><br /> **datetime** = 3<br /><br /> Tous les autres types = 0|**Int**<br /><br /> NULL = Entrée non valide.|  
|**TotalBytes**|Nombre d'octets requis pour conserver les métadonnées et les données de la valeur. Ces informations permettent de vérifier la taille maximale des données dans une colonne **sql_variant**. Si cette valeur est supérieure à 900, la création de l’index échoue.|**Int**<br /><br /> NULL = Entrée non valide.|  
|**Classement**|Représente le classement de la valeur **sql_variant** particulière.|**sysname**<br /><br /> NULL = Entrée non valide.|  
|**MaxLength**|Longueur maximale du type de données (en octets). Par exemple, **MaxLength** de **nvarchar(** 50 **)** est 100, **MaxLength** de **int** est 4.|**Int**<br /><br /> NULL = Entrée non valide.|  
  
## <a name="return-types"></a>Types de retour  
 **sql_variant**  
  
## <a name="examples"></a>Exemples  
### <a name="a-using-a-sqlvariant-in-a-table"></a>A. Utilisation d’un type sql_variant dans une table  
 L’exemple suivant extrait des informations `SQL_VARIANT_PROPERTY` relatives à la valeur `colA` `46279.1` où `colB` =`1689`, étant donné que `tableA` a la valeur `colA` de type `sql_variant` et `colB`.  
  
```sql    
CREATE   TABLE tableA(colA sql_variant, colB int)  
INSERT INTO tableA values ( cast (46279.1 as decimal(8,2)), 1689)  
SELECT   SQL_VARIANT_PROPERTY(colA,'BaseType') AS 'Base Type',  
         SQL_VARIANT_PROPERTY(colA,'Precision') AS 'Precision',  
         SQL_VARIANT_PROPERTY(colA,'Scale') AS 'Scale'  
FROM      tableA  
WHERE      colB = 1689  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)] Notez que chacune de ces trois valeurs est de type **sql_variant**.  
  
```  
Base Type    Precision    Scale  
---------    ---------    -----  
decimal      8           2  
  
(1 row(s) affected)  
```  
  
### <a name="b-using-a-sqlvariant-as-a-variable"></a>B. Utilisation d’un type sql_variant comme variable   
 L’exemple suivant extrait les informations `SQL_VARIANT_PROPERTY` relatives à une variable nommée @v1.  
  
```sql    
DECLARE @v1 sql_variant;  
SET @v1 = 'ABC';  
SELECT @v1;  
SELECT SQL_VARIANT_PROPERTY(@v1, 'BaseType');  
SELECT SQL_VARIANT_PROPERTY(@v1, 'MaxLength');  
```  
  
## <a name="see-also"></a> Voir aussi  
 [sql_variant &#40;Transact-SQL&#41;](../../t-sql/data-types/sql-variant-transact-sql.md)  
  
  

