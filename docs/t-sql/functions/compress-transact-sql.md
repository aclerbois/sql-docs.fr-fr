---
title: COMPRESS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: t-sql|functions
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: reference
f1_keywords:
- COMPRESS
- COMPRESS_TSQL
helpviewer_keywords:
- COMPRESS function
ms.assetid: c2bfe9b8-57a4-48b4-b028-e1a3ed5ece88
caps.latest.revision: 9
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 893790448524a844b488478230db0f5141173ffd
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="compress-transact-sql"></a>COMPRESS (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2016-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2016-asdb-xxxx-xxx-md.md)]

Cette fonction compresse l’expression d’entrée à l’aide de l’algorithme GZIP. La fonction retourne un tableau d’octets de type **varbinary(max)**.
  
![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntaxe  
  
```sql
COMPRESS ( expression )  
```  
  
## <a name="arguments"></a>Arguments  
*expression*  
Un

* **binary(***n***)**
* **char(***n***)**
* **nchar(***n***)**
* **nvarchar(max)**
* **nvarchar(***n***)**
* **varbinary(max)**
* **varbinary(***n***)**
* **varchar(max)**

ou Gestionnaire de configuration

* **varchar(***n***)**

expression. Pour plus d’informations, consultez [Expressions &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md).
  
## <a name="return-types"></a>Types de retour
**varbinary(max)** représentant le contenu compressé de l’entrée.
  
## <a name="remarks"></a>Notes   
Les données compressées ne peuvent pas être indexées.
  
La fonction `COMPRESS` compresse les données d’expression d’entrée. Vous devez appeler cette fonction pour chaque section de données à compresser. Consultez [Compression des données](../../relational-databases/data-compression/data-compression.md) pour plus d’informations sur la compression automatique des données pendant le stockage au niveau des lignes ou des pages.
  
## <a name="examples"></a>Exemples  
  
### <a name="a-compress-data-during-the-table-insert"></a>A. Compresser les données durant l’insertion dans la table  
Cet exemple montre comment compresser les données insérées dans une table :
  
```sql
INSERT INTO player (name, surname, info )  
VALUES (N'Ovidiu', N'Cracium',   
        COMPRESS(N'{"sport":"Tennis","age": 28,"rank":1,"points":15258, turn":17}'));  
  
INSERT INTO player (name, surname, info )  
VALUES (N'Michael', N'Raheem', compress(@info));  
```  
  
### <a name="b-archive-compressed-version-of-deleted-rows"></a>B. Archiver la version compressée des lignes supprimées  
Cette instruction commence par supprimer les anciens enregistrements de lecteur de la table `player`. Pour gagner de l’espace, elle stocke les enregistrements dans la table `inactivePlayer`, dans un format compressé.
  
```sql
DELETE player  
WHERE datemodified < @startOfYear  
OUTPUT id, name, surname datemodifier, COMPRESS(info)   
INTO dbo.inactivePlayers ;  
```  
  
## <a name="see-also"></a>Voir aussi
[Fonctions de chaîne &#40;Transact-SQL&#41;](../../t-sql/functions/string-functions-transact-sql.md)  
[DECOMPRESS &#40;Transact-SQL&#41;](../../t-sql/functions/decompress-transact-sql.md)
  
  
