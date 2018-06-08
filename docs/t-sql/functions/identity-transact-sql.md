---
title: '@@IDENTITY (Transact-SQL) | Microsoft Docs'
ms.custom: ''
ms.date: 08/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: t-sql|functions
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- '@@IDENTITY_TSQL'
- '@@IDENTITY'
dev_langs:
- TSQL
helpviewer_keywords:
- last-inserted identity values
- identity values [SQL Server], last-inserted
- '@@IDENTITY function'
ms.assetid: 912e4485-683c-41c2-97b3-8831c0289ee4
caps.latest.revision: 35
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: cc55633e0af348d226566cbf0bd981185efef2fb
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="x40x40identity-transact-sql"></a>&#x40;&#x40;IDENTITY (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Fonction système qui retourne la dernière valeur d'identité insérée.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
@@IDENTITY  
```  
  
## <a name="return-types"></a>Types de retour  
 **numeric(38,0)**  
  
## <a name="remarks"></a>Notes   
 À la fin d’une instruction INSERT, SELECT INTO ou de copie par bloc, @@IDENTITY contient la dernière valeur d’identité générée par l’instruction. Si l’instruction n’a affecté aucune table contenant des colonnes d’identité, @@IDENTITY retourne la valeur NULL. Si plusieurs lignes ont été insérées, générant ainsi plusieurs valeurs d’identité, @@IDENTITY retourne la dernière valeur d’identité générée. Si l’instruction active un ou plusieurs déclencheurs chargés d’effectuer des instructions INSERT qui génèrent des valeurs d’identité, l’appel de @@IDENTITY immédiatement après l’instruction retourne la dernière valeur d’identité générée par les déclencheurs. Si un déclencheur est activé après une action d’insertion sur une table dotée d’une colonne d’identité et qu’il effectue une opération d’insertion dans une autre table dépourvue d’une colonne d’identité, @@IDENTITY retourne la valeur d’identité de la première insertion. La valeur @@IDENTITY ne revient pas à une valeur précédente en cas d’échec de l’instruction INSERT ou SELECT INTO, d’échec d’une copie par bloc ou d’une restauration de la transaction.  
  
 Les instructions et les transactions en échec peuvent modifier l'identité actuelle d'une table et créer des trous dans les valeurs des colonnes d'identité. La valeur d'identité n'est jamais annulée, même si la transaction qui a essayé d'insérer la valeur dans la table n'est pas validée. Par exemple, si une instruction INSERT échoue à cause d'une violation d'identité IGNORE_DUP_KEY, la valeur d'identité actuelle de la table augmente quand même d'une unité.  
  
 @@IDENTITY, SCOPE_IDENTITY et IDENT_CURRENT sont des fonctions similaires car elles retournent toutes la dernière valeur insérée dans la colonne IDENTITY d’une table.  
  
 @@IDENTITY et SCOPE_IDENTITY retournent la dernière valeur d’identité générée dans une table au cours de la session en cours. Toutefois, SCOPE_IDENTITY retourne uniquement la valeur à l’intérieur de l’étendue actuelle ; @@IDENTITY n’est pas limitée à une étendue spécifique.  
  
 IDENT_CURRENT n'est pas limitée par l'étendue et par la session ; elle est limitée à une table spécifiée. IDENT_CURRENT retourne la valeur d'identité générée pour une table spécifique dans n'importe quelle session et dans n'importe quelle portée. Pour plus d’informations, consultez [IDENT_CURRENT &#40;Transact-SQL&#41;](../../t-sql/functions/ident-current-transact-sql.md).  
  
 L’étendue de la fonction @@IDENTITY correspond à la session en cours sur le serveur local où elle est exécutée. Cette fonction est inapplicable aux serveurs distants ou liés. Pour obtenir une valeur d'identité sur un autre serveur, exécutez une procédure stockée sur ce serveur distant ou lié puis faites en sorte que celle-ci, en cours d'exécution dans le contexte du serveur distant ou lié, collecte la valeur d'identité et la retourne à la connexion appelante sur le serveur local.  
  
 La réplication peut affecter la valeur @@IDENTITY car elle est utilisée dans les déclencheurs de réplication et les procédures stockées. @@IDENTITY n’est pas un indicateur fiable de l’identité créée par l’utilisateur la plus récente si la colonne fait partie d’un article de réplication. Vous pouvez utiliser la syntaxe de la fonction SCOPE_IDENTITY() à la place de @@IDENTITY. Pour plus d’informations, consultez [SCOPE_IDENTITY &#40;Transact-SQL&#41;](../../t-sql/functions/scope-identity-transact-sql.md)  
  
> [!NOTE]  
>  La procédure stockée ou l’instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] appelante doit être réécrite pour utiliser la fonction `SCOPE_IDENTITY()` qui retourne l’identité la plus récente utilisée dans l’étendue de cette instruction utilisateur, et non l’identité dans l’étendue du déclencheur imbriqué utilisée par la réplication.  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant insère une ligne dans une table dotée d'une colonne d'identité (`LocationID`) et utilise `@@IDENTITY` pour afficher la valeur d'identité utilisée dans la nouvelle ligne.  
  
```  
USE AdventureWorks2012;  
GO  
--Display the value of LocationID in the last row in the table.  
SELECT MAX(LocationID) FROM Production.Location;  
GO  
INSERT INTO Production.Location (Name, CostRate, Availability, ModifiedDate)  
VALUES ('Damaged Goods', 5, 2.5, GETDATE());  
GO  
SELECT @@IDENTITY AS 'Identity';  
GO  
--Display the value of LocationID of the newly inserted row.  
SELECT MAX(LocationID) FROM Production.Location;  
GO  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Fonctions système &#40;Transact-SQL&#41;](../../relational-databases/system-functions/system-functions-for-transact-sql.md)   
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
 [IDENT_CURRENT &#40;Transact-SQL&#41;](../../t-sql/functions/ident-current-transact-sql.md)   
 [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)   
 [SCOPE_IDENTITY &#40;Transact-SQL&#41;](../../t-sql/functions/scope-identity-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)  
  
  
