---
title: Conversion de types de données (moteur de base de données) | Microsoft Docs
ms.custom: ''
ms.date: 7/23/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: t-sql|data-types
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- CAST function
- converting data types [SQL Server]
- CONVERT function
- data types [SQL Server], converting
- implicit data type conversions
- explicit data type conversions [SQL Server]
- converting data types [SQL Server], about converting data types
ms.assetid: ffacf45e-a488-48d0-9bb0-dcc7fd365299
caps.latest.revision: 37
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 3a293fa97d10d08ce86714af6bb8ed27b10e2f78
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="data-type-conversion-database-engine"></a>Conversion de types de données (moteur de base de données)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

Les types de données peuvent être convertis dans les scénarios suivants :
-   Lorsque les données d'un objet sont déplacées dans les données d'un autre objet, sont comparées ou combinées à ces dernières, les données d'un objet doivent être converties du type de données d'un objet en type de données de l'autre.  
-   Quand les données d’une colonne de résultats, d’un code de retour ou d’un paramètre de sortie [!INCLUDE[tsql](../../includes/tsql-md.md)] sont déplacées dans une variable de programme, elles doivent être converties du type de données système [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vers le type de données de la variable.  
  
Lors de la conversion entre une variable d'application et une colonne du jeu de résultats, un code de retour, un paramètre ou un marqueur de paramètre [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], les conversions de types de données acceptées sont définies par l'API de base de données.
  
## <a name="implicit-and-explicit-conversion"></a>Conversions implicites et explicites
Les types de données peuvent être convertis implicitement ou explicitement.
  
Les conversions implicites sont invisibles pour l'utilisateur. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] convertit automatiquement les données d'un type de données en un autre. Par exemple, quand un **smallint** est comparé à un **int**, le **smallint** est implicitement converti en **int** avant de poursuivre la comparaison.
  
**GETDATE()** convertit implicitement la date en style 0. **SYSDATETIME()** convertit implicitement la date en style 21.
  
Les conversions explicites utilisent les fonctions CAST ou CONVERT.
  
Les fonctions [CAST et CONVERT](../../t-sql/functions/cast-and-convert-transact-sql.md) convertissent une valeur (une variable locale, une colonne ou une autre expression) d’un type de données en un autre. Par exemple, la fonction `CAST` convertit la valeur numérique de `$157.27` en une chaîne de caractères `'157.27'` :
  
```sql
CAST ( $157.27 AS VARCHAR(10) )  
```  
  
Utilisez CAST au lieu de CONVERT si vous souhaitez que le code de programmation [!INCLUDE[tsql](../../includes/tsql-md.md)] soit compatible avec la norme ISO. Utilisez la fonction CONVERT et non la fonction CAST pour bénéficier de la fonctionnalité style de la fonction CONVERT.
  
L'illustration ci-dessous reprend toutes les conversions de types de données explicites et implicites autorisées pour les types de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournis par le système. Ces types sont notamment **xml**, **bigint** et **sql_variant**. Aucune conversion implicite d’attribution de valeur n’est effectuée à partir du type de données **sql_variant**, mais une conversion implicite vers **sql_variant** existe.
  
![Table de conversion de types de données](../../t-sql/data-types/media/lrdatahd.png "Table de conversion de types de données")
  
## <a name="data-type-conversion-behaviors"></a>Comportements de conversion de types de données
Lors de la conversion du type de données d'un objet [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en un autre, certaines conversions de types de données implicites et explicites ne sont pas prises en charge. Ainsi, une valeur **nchar** ne peut pas être convertie en valeur **image**. Une valeur **nchar** peut uniquement être convertie en **binary** avec une conversion explicite ; une conversion implicite en **binary** n’est pas prise en charge. Cependant, une valeur **nchar** peut être convertie explicitement ou implicitement en **nvarchar**.
  
Les rubriques suivantes décrivent les comportements de conversion propres aux types de données correspondants :
  
 - [binary et varbinary &#40;Transact-SQL&#41;](../../t-sql/data-types/binary-and-varbinary-transact-sql.md)  
 - [datetime2 &#40;Transact-SQL&#41;](../../t-sql/data-types/datetime2-transact-sql.md)  
 - [money et smallmoney &#40;Transact-SQL&#41;](../../t-sql/data-types/money-and-smallmoney-transact-sql.md)  
 - [bit &#40;Transact-SQL&#41;](../../t-sql/data-types/bit-transact-sql.md)  
 - [datetimeoffset &#40;Transact-SQL&#41;](../../t-sql/data-types/datetimeoffset-transact-sql.md)  
 - [smalldatetime &#40;Transact-SQL&#41;](../../t-sql/data-types/smalldatetime-transact-sql.md)  
 - [char et varchar &#40;Transact-SQL&#41;](../../t-sql/data-types/char-and-varchar-transact-sql.md)  
 - [decimal et numeric &#40;Transact-SQL&#41;](../../t-sql/data-types/decimal-and-numeric-transact-sql.md)  
 - [sql_variant &#40;Transact-SQL&#41;](../../t-sql/data-types/sql-variant-transact-sql.md)  
 - [date &#40;Transact-SQL&#41;](../../t-sql/data-types/date-transact-sql.md)  
 - [float et real &#40;Transact-SQL&#41;](../../t-sql/data-types/float-and-real-transact-sql.md)  
 - [time &#40;Transact-SQL&#41;](../../t-sql/data-types/time-transact-sql.md)  
 - [datetime &#40;Transact-SQL&#41;](../../t-sql/data-types/datetime-transact-sql.md)  
 - [int, bigint, smallint et tinyint &#40;Transact-SQL&#41;](../../t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql.md)  
 - [uniqueidentifier &#40;Transact-SQL&#41;](../../t-sql/data-types/uniqueidentifier-transact-sql.md)  
  
###  <a name="converting-data-types-by-using-ole-automation-stored-procedures"></a>Conversion de types de données à l'aide des procédures stockées OLE Automation  
Étant donné que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise les types de données [!INCLUDE[tsql](../../includes/tsql-md.md)] et qu'OLE Automation utilise les types de données [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)], les procédures stockées OLE Automation doivent convertir les données qu'elles s'échangent.
  
Le tableau suivant décrit la conversion des types de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en types de données [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)].
  
|Type de données SQL Server|Type de données Visual Basic|  
|--------------------------|----------------------------|  
|**char**, **varchar**, **text**, **nvarchar**, **ntext**|**String**|  
|**decimal**, **numeric**|**String**|  
|**bit**|**Booléen**|  
|**binary**, **varbinary**, **image**|Tableau de type **Byte()** unidimensionnel|  
|**Int**|**Long**|  
|**smallint**|**Integer**|  
|**tinyint**|**Byte**|  
|**float**|**Double**|  
|**real**|**Unique**|  
|**money**, **smallmoney**|**Monétaire (Currency)**|  
|**datetime**, **smalldatetime**|**Date**|  
|Tout type qui prend la valeur NULL|**Variant** ayant la valeur Null|  
  
Toutes les valeurs uniques [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont converties en une valeur unique [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)], à l’exception des valeurs **binary**, **varbinary** et **image**. Ces valeurs sont converties en tableau de type **Byte()** unidimensionnel dans [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)]. Ce tableau possède une plage **Byte(** 0 à *length*1 **)** où *length* représente le nombre d’octets dans les valeurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **binary**, **varbinary** ou **image**.
  
Il s'agit des conversions des types de données [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] en types de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
  
|Type de données Visual Basic|Type de données SQL Server|  
|----------------------------|--------------------------|  
|**Long**, **Integer**, **Byte**, **Boolean**, **Object**|**Int**|  
|**Double**, **Single**|**float**|  
|**Monétaire (Currency)**|**money**|  
|**Date**|**datetime**|  
|**String** avec 4 000 caractères au maximum|**varchar**/**nvarchar**|  
|**String** avec plus de 4 000 caractères|**text**/**ntext**|  
|Tableau de type **Byte()** unidimensionnel avec 8 000 octets au maximum|**varbinary**|  
|Tableau de type **Byte()** unidimensionnel avec plus de 8 000 octets|**image**|  
  
## <a name="see-also"></a>Voir aussi
[Procédures stockées OLE Automation &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/ole-automation-stored-procedures-transact-sql.md)  
[CAST et CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)  
[Types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)  
[COLLATE &#40;Transact-SQL&#41;](http://msdn.microsoft.com/library/4ba6b7d8-114a-4f4e-bb38-fe5697add4e9)
  
  
