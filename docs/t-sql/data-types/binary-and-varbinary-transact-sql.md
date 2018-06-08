---
title: binary et varbinary (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 8/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: t-sql|data-types
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- binary_TSQL
- varbinary_TSQL
- binary
- varbinary
dev_langs:
- TSQL
helpviewer_keywords:
- varbinary data type
- binary [SQL Server], about binary data type
ms.assetid: bcce65f9-10db-4b3e-bfaf-dfc06c6f820f
caps.latest.revision: 27
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 624a2b764d6c397b950796fe822764b7983664ae
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="binary-and-varbinary-transact-sql"></a>binary et varbinary (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

Données de type binaire de longueur fixe (binary) ou variable (varbinary).
  
## <a name="arguments"></a>Arguments  
**binary** [ ( *n* ) ] Données binaires de longueur fixe de *n* octets, où *n* est une valeur comprise entre 1 et 8 000. La taille de stockage est égale à *n* octets.
  
**varbinary** [ ( *n* | **max**) ] Données binaires de longueur variable. *n* peut être une valeur comprise entre 1 et 8 000. **max** indique que la taille de stockage maximale est de 2^31-1 octets. La taille mémoire est la longueur réelle des données entrées, plus deux octets. Les données entrées peuvent avoir une longueur de 0 octet. Le synonyme SQL ANSI de **varbinary** est **binary varying**.
  
## <a name="remarks"></a>Notes   
Quand la valeur de *n* n’est spécifiée ni dans une définition de données ni dans une instruction de déclaration de variable, la longueur par défaut est 1. Quand la valeur de *n* n’est pas précisée avec la fonction CAST, la longueur par défaut est 30.

| Type de données | Utilisation quand... |
| --- | --- |
| **binaire** | les tailles des entrées de données de la colonne sont cohérentes.|
| **varbinary** | les tailles des entrées de données de la colonne varient considérablement.|
| **varbinary(max)** | la taille des entrées de données de la colonne dépasse 8 000 octets.|


## <a name="converting-binary-and-varbinary-data"></a>Conversion de données binary et varbinary
Quand les données sont converties à partir d’un type de données string (**char**, **varchar**, **nchar**, **nvarchar**, **binary**, **varbinary**, **text**, **ntext** ou **image**) en un type de données **binary** ou **varbinary** de longueur différente, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] complète ou tronque les données sur la droite. Quand d’autres types de données sont convertis en **binary** ou **varbinary**, les données sont complétées ou tronquées à gauche. Elles sont complétées avec des zéros hexadécimaux.
  
La conversion de données en types de données **binary** et **varbinary** est utile si les données **binary** constituent le moyen de déplacement de données le plus pratique. La conversion d’une valeur d’un type quelconque en valeur binaire de taille suffisante, puis sa reconversion en son type d’origine produisent toujours la même valeur si les deux conversions sont effectuées dans la même version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La représentation binaire d'une valeur peut varier d'une version à l'autre de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
  
Vous pouvez convertir **int**, **smallint** et **tinyint** en **binary** ou **varbinary** mais, si vous reconvertissez la valeur **binary** en valeur entière, elle sera différente de la valeur entière initiale en cas de troncation. Par exemple, l'instruction SELECT suivante montre que la valeur entière `123456` est généralement stockée sous forme binaire `0x0001e240` :
  
```sql
SELECT CAST( 123456 AS BINARY(4) );  
```  
  
Cependant, l’instruction `SELECT` suivante montre que si la cible **binary** est trop petite pour traiter la valeur entière, les chiffres les plus à gauche sont tronqués discrètement de façon à ce que le même nombre soit stocké en tant que `0xe240` :
  
```sql
SELECT CAST( 123456 AS BINARY(2) );  
```  
  
L'instruction suivante montre que la troncation discrète peut affecter les opérations arithmétiques sans engendrer d'erreur :
  
```sql
DECLARE @BinaryVariable2 BINARY(2);  
  
SET @BinaryVariable2 = 123456;  
SET @BinaryVariable2 = @BinaryVariable2 + 1;  
  
SELECT CAST( @BinaryVariable2 AS INT);  
GO  
```  
  
Le résultat final est `57921`, et non pas `123457`.
  
> [!NOTE]  
>  Les conversions entre un type de données quelconque et les types de données **binary** ne sont pas nécessairement identiques dans toutes les versions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="see-also"></a>Voir aussi
[CAST et CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)  
[Conversion de type de données &#40;moteur de base de données&#41;](../../t-sql/data-types/data-type-conversion-database-engine.md)  
[Types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)
  
  
