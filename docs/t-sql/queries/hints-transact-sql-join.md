---
title: Indicateurs de jointure (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: t-sql|queries
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- Join Hint
- Join_Hint_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- HASH join hint
- REMOTE join hint
- LOOP join hint
- join hints
- MERGE join hint
- hints [SQL Server], join
ms.assetid: 09069f4a-f2e3-4717-80e1-c0110058efc4
caps.latest.revision: 30
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: b350421cd40a23f6fdebefa797b22367f1ef6a58
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="hints-transact-sql---join"></a>Indicateurs (Transact-SQL) - Jointure
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Les indicateurs de jointure spécifient l'application, par l'optimiseur de requête, d'une stratégie de jointure entre deux tables dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]. Pour obtenir des informations générales sur les jointures et la syntaxe de jointure, consultez [FROM &#40;Transact-SQL&#41;](../../t-sql/queries/from-transact-sql.md).  
  
> [!IMPORTANT]  
>  Étant donné que l’optimiseur de requête [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sélectionne généralement le meilleur plan d’exécution pour une requête, nous vous recommandons d’utiliser les indicateurs, y compris \<join_hint>, uniquement en dernier recours et si vous êtes un administrateur de base de données ou un développeur expérimenté.
  
 **S’applique à :**  
  
 [DELETE](../../t-sql/statements/delete-transact-sql.md)  
  
 [SELECT](../../t-sql/queries/select-transact-sql.md)  
  
 [UPDATE](../../t-sql/queries/update-transact-sql.md)  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
<join_hint> ::=   
     { LOOP | HASH | MERGE | REMOTE }  
```  
  
## <a name="arguments"></a>Arguments  
 LOOP | HASH | MERGE  
 Spécifie que la jointure dans la requête doit utiliser les boucles, le hachage ou la fusion. L'utilisation des arguments LOOP | HASH | MERGE JOIN applique un type de jointure particulier entre deux tables. LOOP ne peut pas être spécifié en même temps que RIGHT ou FULL en tant que type de jointure.  
  
 REMOTE  
 Effectue une opération de jointure sur le site de la table de droite. Ceci est utile lorsque la table de gauche est une table locale et que celle de droite est une table distante. L'argument REMOTE ne doit être utilisé que lorsque la table de gauche possède moins de lignes que celle de droite.  
  
 Si la table de droite est une table locale, la jointure sera effectuée en local. Si les deux tables sont des tables distantes mais qu'elles proviennent de sources de données différentes, REMOTE provoque l'exécution de la jointure sur le site de la table de droite. Si les deux tables sont des tables distantes provenant de la même source de données, REMOTE n'est pas nécessaire.  
  
 REMOTE ne peut pas être utilisé lorsque l'une des valeurs comparées dans le prédicat de jointure est affecté à un autre classement à l'aide de la clause COLLATE.  
  
 REMOTE ne peut être utilisé que pour les opérations INNER JOIN.  
  
## <a name="remarks"></a>Notes   
 Les indicateurs de jointure sont spécifiés dans la clause FROM d'une requête. Les indicateurs de jointure appliquent une stratégie de jointure entre deux tables. Si un indicateur de jointure est spécifié pour deux tables, l'optimiseur de requêtes applique automatiquement l'ordre de jointure de toutes les tables jointes de la requête, d'après la position des mots clés ON. Lorsqu'une jointure CROSS JOIN est utilisée sans la clause ON, l'ordre de jointure peut être indiqué au moyen de parenthèses.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-hash"></a>A. Utilisation d'HASH  
 L'exemple suivant spécifie que l'opération `JOIN` figurant dans la requête est effectuée par une jointure `HASH`. L'exemple utilise la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```  
SELECT p.Name, pr.ProductReviewID  
FROM Production.Product AS p  
LEFT OUTER HASH JOIN Production.ProductReview AS pr  
ON p.ProductID = pr.ProductID  
ORDER BY ProductReviewID DESC;  
```  
  
### <a name="b-using-loop"></a>B. Utilisation d'LOOP  
 L'exemple suivant spécifie que l'opération `JOIN` figurant dans la requête est effectuée par une jointure `LOOP`. L'exemple utilise la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```  
DELETE FROM Sales.SalesPersonQuotaHistory   
FROM Sales.SalesPersonQuotaHistory AS spqh  
    INNER LOOP JOIN Sales.SalesPerson AS sp  
    ON spqh.SalesPersonID = sp.SalesPersonID  
WHERE sp.SalesYTD > 2500000.00;  
GO  
```  
  
### <a name="c-using-merge"></a>C. Utilisation de MERGE  
 L'exemple suivant spécifie que l'opération `JOIN` figurant dans la requête est effectuée par une jointure `MERGE`. L'exemple utilise la base de données [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```  
SELECT poh.PurchaseOrderID, poh.OrderDate, pod.ProductID, pod.DueDate, poh.VendorID   
FROM Purchasing.PurchaseOrderHeader AS poh  
INNER MERGE JOIN Purchasing.PurchaseOrderDetail AS pod   
    ON poh.PurchaseOrderID = pod.PurchaseOrderID;  
GO  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Indicateurs &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql.md)  
  
  
