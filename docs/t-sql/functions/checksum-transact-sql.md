---
title: CHECKSUM (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: sql-data-warehouse, database-engine, sql-database
ms.component: t-sql|functions
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- CHECKSUM_TSQL
- CHECKSUM
dev_langs:
- TSQL
helpviewer_keywords:
- hash indexes
- CHECKSUM function
- checksum values
ms.assetid: e26d3339-845c-49c2-9d89-243376874c13
caps.latest.revision: 44
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 1049f451548d5bd8bb7dbae705830802cc0c13b7
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="checksum-transact-sql"></a>CHECKSUM (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-asdw-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-asdw-xxx-md.md)]

La fonction `CHECKSUM` retourne la valeur de somme de contrôle calculée à partir de la ligne d’une table ou à partir d’une liste d’expressions. Utilisez `CHECKSUM` pour générer des index de hachage.
  
![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntaxe  
  
```sql
CHECKSUM ( * | expression [ ,...n ] )  
```  
  
## <a name="arguments"></a>Arguments  
\*  
Cet argument spécifie que le calcul de la somme de contrôle couvre toutes les colonnes de la table. `CHECKSUM` retourne une erreur si une colonne est d’un type de données incomparable. Les types de données incomparables sont notamment :

- **cursor**
- **image**
- **ntext**
- **texte**
- **XML**

Un autre type de données incomparable est **sql_variant** avec l’un des types précédents comme type de base.
  
*expression*  
[Expression](../../t-sql/language-elements/expressions-transact-sql.md) de type quelconque, à l’exception d’un type de données incomparable.
  
## <a name="return-types"></a>Types de retour
 **Int**  
  
## <a name="remarks"></a>Notes   
CHECKSUM calcule une valeur de hachage, appelée somme de contrôle, sur sa liste d’arguments. Utilisez cette valeur de hachage pour générer des index de hachage. Un index de hachage est obtenu si la fonction `CHECKSUM` a des arguments de colonne et qu’un index est créé sur la valeur CHECKSUM calculée. qui peut être utilisé dans des recherches d'égalité sur les colonnes.
  
La fonction `CHECKSUM` a les propriétés d’une fonction de hachage : quand `CHECKSUM` est appliqué à deux listes d’expressions, la même valeur est retournée si les éléments correspondants dans les deux listes sont du même type de données et ont une valeur égale lorsqu’ils sont comparés à l’aide de l’opérateur d’égalité (=). Les valeurs NULL d’un type spécifié sont définies comme ayant une valeur de comparaison égale dans le cadre de la fonction `CHECKSUM`. Si au moins l’une des valeurs de la liste d’expressions change, la somme de contrôle de liste est susceptible de changer aussi. Toutefois, cela n’est pas garanti. Par conséquent, nous conseillons d’utiliser `CHECKSUM` pour vérifier si des valeurs ont changé uniquement si votre application peut accepter une modification parfois manquée. Sinon, envisagez d’utiliser [HashBytes](../../t-sql/functions/hashbytes-transact-sql.md) à la place. Avec un algorithme de hachage MD5 spécifié, la probabilité que HashBytes retourne le même résultat pour deux entrées différentes est beaucoup plus faible par rapport à CHECKSUM.
  
L’ordre des expressions affecte la valeur `CHECKSUM` calculée. L’ordre des colonnes utilisé pour CHECKSUM(\*) est celui spécifié dans la définition de la table ou de la vue, y compris les colonnes calculées.
  
La valeur de CHECKSUM dépend du classement. La même valeur stockée avec un autre classement retourne une valeur CHECKSUM différente.
  
## <a name="examples"></a>Exemples  
Ces exemples illustrent l’utilisation de `CHECKSUM` pour générer des index de hachage.
  
Pour générer l’index de hachage, le premier exemple ajoute une colonne de somme de contrôle calculée à la table à indexer. Il génère ensuite un index sur la colonne de somme de contrôle. 
  
```sql
-- Create a checksum index.  
SET ARITHABORT ON;  
USE AdventureWorks2012;   
GO  
ALTER TABLE Production.Product  
ADD cs_Pname AS CHECKSUM(Name);  
GO  
CREATE INDEX Pname_index ON Production.Product (cs_Pname);  
GO  
```  
  
Cet exemple illustre l’utilisation d’un index de somme de contrôle comme index de hachage. Cela peut permettre d’améliorer la vitesse d’indexation quand la colonne à indexer est une colonne contenant des chaînes de caractères longues. L'index de la somme de contrôle peut être utilisé dans les recherches d'égalité.
  
```sql
/*Use the index in a SELECT query. Add a second search   
condition to catch stray cases where checksums match,   
but the values are not the same.*/  
SELECT *   
FROM Production.Product  
WHERE CHECKSUM(N'Bearing Ball') = cs_Pname  
AND Name = N'Bearing Ball';  
GO  
```  
  
La création d’index sur la colonne calculée matérialise la colonne de somme de contrôle, et toutes les modifications apportées à la valeur `ProductName` sont propagées à cette colonne. Nous pourrions également créer un index directement sur la colonne à indexer. Toutefois, pour les valeurs de clé longues, un index normal n’est probablement pas aussi performant qu’un index de somme de contrôle.
  
## <a name="see-also"></a>Voir aussi
[CHECKSUM_AGG &#40;Transact-SQL&#41;](../../t-sql/functions/checksum-agg-transact-sql.md)  
[HASHBYTES &#40;Transact-SQL&#41;](../../t-sql/functions/hashbytes-transact-sql.md)  
[BINARY_CHECKSUM  &#40;Transact-SQL&#41;](../../t-sql/functions/binary-checksum-transact-sql.md)
  
  
