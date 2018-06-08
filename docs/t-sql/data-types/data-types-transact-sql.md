---
title: Types de données (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 9/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: t-sql|data-types
ms.reviewer: ''
ms.suite: sql
ms.technology: database-engine
ms.tgt_pltfrm: ''
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- system data types [SQL Server]
- data types [SQL Server]
- data types [SQL Server], about data types
ms.assetid: a54f7373-b247-4d61-8fb8-7f2ec7a8d0a4
caps.latest.revision: 45
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 7e811528caf2df4930dc619c8c7eed8438906ab6
ms.sourcegitcommit: 808d23a654ef03ea16db1aa23edab496b73e5072
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34708847"
---
# <a name="data-types-transact-sql"></a>Types de données (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], à chaque colonne, variable locale, expression et paramètre correspond un type de données. Un type de données est un attribut qui spécifie le type de données que l'objet peut contenir : données de type Integer, données caractères, données monétaires, données de date et d'heure, chaînes binaires, et ainsi de suite.
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournit un ensemble de types de données système qui définissent tous les types de données utilisables avec [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Vous pouvez également définir vos propres types de données dans [!INCLUDE[tsql](../../includes/tsql-md.md)] ou [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)]. Les types de données d'alias sont basés sur les types de données fournis par le système. Pour plus d’informations sur les types de données d’alias, consultez [CREATE TYPE &#40;Transact-SQL&#41;](../../t-sql/statements/create-type-transact-sql.md). Les types définis par l'utilisateur tirent leurs caractéristiques des méthodes et des opérateurs d'une classe que vous créez à l'aide de l'un des langages de programmation pris en charge par [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)].
  
Lorsqu'un opérateur combine deux expressions qui diffèrent par les types de données, les classements, la précision, l'échelle ou la longueur, les caractéristiques du résultat sont déterminées comme suit :
-   Le type de données du résultat est déterminé par l'application des règles de priorité des types de données aux types de données des expressions entrées. Pour plus d’informations, consultez [Priorités des types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-type-precedence-transact-sql.md).  
-   Le classement du résultat est déterminé par les règles de priorité de classement quand le type de données du résultat est **char**, **varchar**, **text**, **nchar**, **nvarchar** ou **ntext**. Pour plus d’informations, consultez [Priorité de classement &#40;Transact-SQL&#41;](../../t-sql/statements/collation-precedence-transact-sql.md).  
-   Les précision, échelle et longueur du résultat dépendent des précision, échelle et longueur des expressions entrées. Pour plus d’informations, consultez [Précision, échelle et longueur &#40;Transact-SQL&#41;](../../t-sql/data-types/precision-scale-and-length-transact-sql.md).  
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] propose des synonymes de types de données pour la compatibilité ISO. Pour plus d’informations, consultez [Synonymes des types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-type-synonyms-transact-sql.md).
  
## <a name="data-type-categories"></a>Catégories de types de données
Les catégories des types de données de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont les suivantes :
  
|||  
|-|-|  
|Valeurs numériques exactes|Chaînes de caractères Unicode|  
|Valeurs numériques approximatives|Chaînes binaires|  
|Date et heure|Autres types de données|  
|Chaînes de caractères||  
  
Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], en fonction de leurs caractéristiques de stockage, certains types de données sont désignés comme appartenant aux groupes suivants :
-   Types de données de valeur élevée : **varchar(max)** et **nvarchar(max)**  
-   Types de données LOB : **text**, **ntext**, **image**, **varbinary(max)** et **xml**  
  
    > [!NOTE]  
    >  sp_help retourne -1 comme longueur des types de données de valeur élevée et **xml**.  
  
### <a name="exact-numerics"></a>Valeurs numériques exactes
  
|||  
|-|-|  
|[bigint](../../t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql.md)|[numeric](../../t-sql/data-types/decimal-and-numeric-transact-sql.md)|  
|[bit](../../t-sql/data-types/bit-transact-sql.md)|[smallint](../../t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql.md)|  
|[decimal](../../t-sql/data-types/decimal-and-numeric-transact-sql.md)|[smallmoney](../../t-sql/data-types/money-and-smallmoney-transact-sql.md)|  
|[Int](../../t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql.md)|[tinyint](../../t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql.md)|  
|[money](../../t-sql/data-types/money-and-smallmoney-transact-sql.md)||  
  
### <a name="approximate-numerics"></a>Valeurs numériques approximatives
  
|||  
|-|-|  
|[float](../../t-sql/data-types/float-and-real-transact-sql.md)|[real](../../t-sql/data-types/float-and-real-transact-sql.md)|  
  
### <a name="date-and-time"></a>Date et heure
  
|||  
|-|-|  
|[date](../../t-sql/data-types/date-transact-sql.md)|[datetimeoffset](../../t-sql/data-types/datetimeoffset-transact-sql.md)|  
|[datetime2](../../t-sql/data-types/datetime2-transact-sql.md)|[smalldatetime](../../t-sql/data-types/smalldatetime-transact-sql.md)|  
|[datetime](../../t-sql/data-types/datetime-transact-sql.md)|[time](../../t-sql/data-types/time-transact-sql.md)|  
  
### <a name="character-strings"></a>Chaînes de caractères
  
|||  
|-|-|  
|[char](../../t-sql/data-types/char-and-varchar-transact-sql.md)|[varchar](../../t-sql/data-types/char-and-varchar-transact-sql.md)|  
|[texte](../../t-sql/data-types/ntext-text-and-image-transact-sql.md)||  
  
### <a name="unicode-character-strings"></a>Chaînes de caractères Unicode
  
|||  
|-|-|  
|[nchar](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md)|[nvarchar](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md)|  
|[ntext](../../t-sql/data-types/ntext-text-and-image-transact-sql.md)||  
  
### <a name="binary-strings"></a>Chaînes binaires
  
|||  
|-|-|  
|[binaire](../../t-sql/data-types/binary-and-varbinary-transact-sql.md)|[varbinary](../../t-sql/data-types/binary-and-varbinary-transact-sql.md)|  
|[image](../../t-sql/data-types/ntext-text-and-image-transact-sql.md)||  
  
### <a name="other-data-types"></a>Autres types de données
  
|||  
|-|-|  
|[cursor](../../t-sql/data-types/cursor-transact-sql.md)|[rowversion](../../t-sql/data-types/rowversion-transact-sql.md)|  
|[hierarchyid](../../t-sql/data-types/hierarchyid-data-type-method-reference.md)|[uniqueidentifier](../../t-sql/data-types/uniqueidentifier-transact-sql.md)|  
|[sql_variant](../../t-sql/data-types/sql-variant-transact-sql.md)|[xml](../../t-sql/xml/xml-transact-sql.md)|  
|[Types geometry spatiaux](../../t-sql/spatial-geometry/spatial-types-geometry-transact-sql.md) |[Types geography spatiaux](../../t-sql/spatial-geography/spatial-types-geography.md)|  
|[table](../../t-sql/data-types/table-transact-sql.md) | |
  
## <a name="see-also"></a>Voir aussi
[CREATE PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/create-procedure-transact-sql.md)  
[CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)  
[DECLARE @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-local-variable-transact-sql.md)
[EXECUTE &#40;Transact-SQL&#41;](../../t-sql/language-elements/execute-transact-sql.md)  
[Expressions &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)  
[Fonctions &#40;Transact-SQL&#41;](../../t-sql/functions/functions.md)  
[LIKE &#40;Transact-SQL&#41;](../../t-sql/language-elements/like-transact-sql.md)  
[sp_droptype &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-droptype-transact-sql.md)  
[sp_help &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-help-transact-sql.md)  
[sp_rename &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-rename-transact-sql.md)
  
  
