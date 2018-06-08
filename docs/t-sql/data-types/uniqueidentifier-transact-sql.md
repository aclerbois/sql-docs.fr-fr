---
title: uniqueidentifier (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/1/2017
ms.prod: sql
ms.prod_service: sql-data-warehouse, database-engine, pdw, sql-database
ms.component: t-sql|data-types
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- uniqueidentifier
- uniqueidentifier_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- uniqueidentifier data type
- globally unique identifiers [SQL Server]
- GUIDs [SQL Server]
ms.assetid: b026035b-f3d2-4d70-989d-3884b4ca0233
caps.latest.revision: 39
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 7128398c7ebec1317cb23dede5565c93ed29b169
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="uniqueidentifier-transact-sql"></a>uniqueidentifier (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-asdw-pdw-md](../../includes/tsql-appliesto-ss2008-asdb-asdw-pdw-md.md)]

GUID sur 16 octets.
  
## <a name="remarks"></a>Notes   
Il existe deux manières d’affecter une valeur initiale à une colonne ou à une variable locale du type de données **uniqueidentifier** :
-   en utilisant les fonctions [NEWID](../../t-sql/functions/newid-transact-sql.md) ou [NEWSEQUENTIALID](../../t-sql/functions/newsequentialid-transact-sql.md) ;    
-   en convertissant à partir d’une constante de chaîne de la forme *xxxxxxxx*-*xxxx*-*xxxx*-*xxxx*-*xxxxxxxxxxxx*, où chaque *x* est un chiffre hexadécimal compris dans la plage 0-9 ou a-f. Par exemple, 6F9619FF-8B86-D011-B42D-00C04FC964FF est une valeur **uniqueidentifier** valide.  
  
Des opérateurs de comparaison peuvent être utilisés avec des valeurs **uniqueidentifier**. Toutefois, le classement n'est pas effectué par la comparaison des configurations binaires des deux valeurs. Les seules opérations que vous êtes autorisé à effectuer sur une valeur **uniqueidentifier** sont les comparaisons (=, <>, \<, >, \<=, >=) et la recherche de valeurs Null (IS NULL et IS NOT NULL). Aucun autre opérateur arithmétique n'est admis. Toutes les propriétés et contraintes de colonnes, à l’exception de la propriété IDENTITY, sont autorisées dans le type de données **uniqueidentifier**.
  
Les réplications de fusion et les réplications transactionnelles possédant des abonnements mis à jour utilisent des colonnes **uniqueidentifier** afin de garantir que les lignes sont identifiées de manière unique au sein des nombreuses copies de la table.
  
## <a name="converting-uniqueidentifier-data"></a>Conversion de données uniqueidentifier  
Le type **uniqueidentifier** est considéré comme un type caractère pour les besoins de la conversion à partir d’une expression de caractères ; il est par conséquent soumis aux règles de troncation pour la conversion en un type caractère. Autrement dit, lorsque des expressions de caractères sont converties en type caractère de taille différente, les valeurs trop longues pour le nouveau type de données sont tronquées. Consultez la section exemples.
  
## <a name="limitations-and-restrictions"></a>Limitations et restrictions

Les outils et fonctionnalités ci-dessous ne prennent pas en charge le type de données `uniqueidentifier` :
- PolyBase
- [Outil de chargement dwloader](https://msdn.microsoft.com/sql/analytics-platform-system/dwloader) pour Parallel Data Warehouse

## <a name="examples"></a>Exemples  
L'exemple suivant convertit une valeur `uniqueidentifier` en type de données `char`.
  
```sql
DECLARE @myid uniqueidentifier = NEWID();  
SELECT CONVERT(char(255), @myid) AS 'char';  
```  
  
L'exemple suivant illustre la troncation des données lorsque la valeur est trop longue pour le type de données vers lequel la conversion est effectuée. Étant donné que le type **uniqueidentifier** est limité à 36 caractères, les caractères qui dépassent cette longueur sont tronqués.
  
```sql
DECLARE @ID nvarchar(max) = N'0E984725-C51C-4BF4-9960-E1C80E27ABA0wrong';  
SELECT @ID, CONVERT(uniqueidentifier, @ID) AS TruncatedValue;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```sql
String                                       TruncatedValue  
-------------------------------------------- ------------------------------------  
0E984725-C51C-4BF4-9960-E1C80E27ABA0wrong    0E984725-C51C-4BF4-9960-E1C80E27ABA0  
  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>Voir aussi
[ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)  
[CAST et CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)  
[CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)  
[Types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)  
[DECLARE @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-local-variable-transact-sql.md)  
[NEWID &#40;Transact-SQL&#41;](../../t-sql/functions/newid-transact-sql.md)  
[NEWSEQUENTIALID &#40;Transact-SQL&#41;](../../t-sql/functions/newsequentialid-transact-sql.md)    
[SET @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/set-local-variable-transact-sql.md)  
[Updatable Subscriptions for Transactional Replication](../../relational-databases/replication/transactional/updatable-subscriptions-for-transactional-replication.md)
  
  
