---
title: CURSOR_STATUS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: t-sql|functions
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- CURSOR_STATUS
- CURSOR_STATUS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- status information [SQL Server], cursors
- CURSOR_STATUS function
- cursors [SQL Server], status information
ms.assetid: 3a4a840e-04f8-43bd-aada-35d78c3cb6b0
caps.latest.revision: 37
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 2bc5eff629bc16e655094c08c71b3cef195420bc
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="cursorstatus-transact-sql"></a>CURSOR_STATUS (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

Pour un paramètre donné, `CURSOR_STATUS` indique si une déclaration de curseur a retourné ou non un curseur et un jeu de résultats.
  
![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Syntaxe  
  
```sql
CURSOR_STATUS   
     (  
          { 'local' , 'cursor_name' }   
          | { 'global' , 'cursor_name' }   
          | { 'variable' , 'cursor_variable' }   
     )  
```  
  
## <a name="arguments"></a>Arguments  
'local'  
Spécifie une constante indiquant que la source du curseur est un nom de curseur local.
  
'*cursor_name*'  
Nom du curseur. Un nom de curseur doit être conforme aux [règles applicables aux identificateurs de base de données](../../relational-databases/databases/database-identifiers.md).
  
'global'  
Spécifie une constante indiquant que la source du curseur est un nom de curseur global.
  
'variable'  
Spécifie une constante indiquant que la source du curseur est une variable locale.
  
'*cursor_variable*'  
Nom de la variable de curseur. Une variable de curseur doit être définie à l’aide du type de données **cursor**.
  
## <a name="return-types"></a>Types de retour
**smallint**
  
|Valeur retournée|Nom du curseur|Variable du curseur|  
|---|---|---|
| 1|Le jeu de résultats de curseur comprend au moins une ligne.<br /><br /> Pour les curseurs INSENSITIVE et pilotés par jeux de clés, l'ensemble de résultats comprend au moins une ligne.<br /><br /> Pour les curseurs dynamiques, l'ensemble de résultats peut être vide, ou contenir une ou plusieurs lignes.|Le curseur affecté à cette variable est ouvert.<br /><br /> Pour les curseurs INSENSITIVE et pilotés par jeux de clés, l'ensemble de résultats comprend au moins une ligne.<br /><br /> Pour les curseurs dynamiques, l'ensemble de résultats peut être vide, ou contenir une ou plusieurs lignes.|  
|0|Le jeu de résultats de curseur est vide.*|Le curseur affecté à cette variable est ouvert mais l'ensemble de résultats est vide.*|  
|-1|Le curseur est fermé.|Le curseur affecté à cette variable est fermé.|  
|-2|Non applicable.|Présente l’une de ces possibilités :<br /><br /> La procédure appelée précédemment n’affectait pas de curseur à cette variable OUTPUT.<br /><br /> La procédure appelée précédemment affectait un curseur à cette variable OUTPUT, mais le curseur était à l’état fermé quand la procédure s’est terminée. C’est la raison pour laquelle le curseur est désaffecté et qu’il n’est pas retourné à la procédure d’appel.<br /><br /> Aucun curseur n’est affecté à la variable de curseur déclarée.|  
|-3|Il n'existe aucun curseur portant le nom spécifié.|Il n’existe aucune variable de curseur avec le nom spécifié ou, si c’est le cas, aucun curseur ne lui a encore été affecté.|  
  
* Les curseurs dynamiques ne retournent jamais cette valeur.
  
## <a name="examples"></a>Exemples  
Cet exemple utilise la fonction `CURSOR_STATUS` pour afficher l’état d’un curseur, après sa déclaration, après son ouverture et après sa fermeture.
  
```sql
CREATE TABLE #TMP  
(  
   ii int  
)  
GO  
  
INSERT INTO #TMP(ii) VALUES(1)  
INSERT INTO #TMP(ii) VALUES(2)  
INSERT INTO #TMP(ii) VALUES(3)  
  
GO  
  
--Create a cursor.  
DECLARE cur CURSOR  
FOR SELECT * FROM #TMP  
  
--Display the status of the cursor before and after opening  
--closing the cursor.  
  
SELECT CURSOR_STATUS('global','cur') AS 'After declare'  
OPEN cur  
SELECT CURSOR_STATUS('global','cur') AS 'After Open'  
CLOSE cur  
SELECT CURSOR_STATUS('global','cur') AS 'After Close'  
  
--Remove the cursor.  
DEALLOCATE cur  
  
--Drop the table.  
DROP TABLE #TMP  
  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
After declare
---------------
-1  
  
After Open
----------
1  
  
After Close
-----------
-1
```  
  
## <a name="see-also"></a>Voir aussi
[Fonctions de curseur &#40;Transact-SQL&#41;](../../t-sql/functions/cursor-functions-transact-sql.md)  
[Types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)
  
  
