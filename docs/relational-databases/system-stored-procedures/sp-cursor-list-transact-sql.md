---
title: sp_cursor_list (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_cursor_list
- sp_cursor_list_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_cursor_list
ms.assetid: 7187cfbe-d4d9-4cfa-a3bb-96a544c7c883
caps.latest.revision: 31
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 368c1b64a7c6eea9d338a1698e5a20d90948b696
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spcursorlist-transact-sql"></a>sp_cursor_list (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Indique les attributs des curseurs côté serveur actuellement ouverts pour la connexion.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_cursor_list [ @cursor_return = ] cursor_variable_name OUTPUT   
     , [ @cursor_scope = ] cursor_scope  
[;]  
```  
  
## <a name="arguments"></a>Arguments  
 [ @cursor_return=] *nom_de_variable_de_curseur*sortie  
 Nom d'une variable de curseur déclarée. *nom_de_variable_de_curseur* est **curseur**, sans valeur par défaut. Le curseur renvoyé est un curseur en lecture seule, dynamique et permettant les défilements.  
  
 [ @cursor_scope=] *étendue_curseur*  
 Spécifie le niveau des curseurs à signaler. *étendue_curseur* est **int**, sans valeur par défaut et peut prendre l’une des valeurs suivantes.  
  
|Valeur| Description|  
|-----------|-----------------|  
|1|Signaler tous les curseurs locaux.|  
|2|Signaler tous les curseurs globaux.|  
|3|Signaler les curseurs locaux et globaux.|  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 Aucun  
  
## <a name="cursors-returned"></a>Curseurs retournés  
 sp_cursor_list renvoie son rapport sous la forme d'un paramètre de résultat de curseur [!INCLUDE[tsql](../../includes/tsql-md.md)] et non pas sous la forme d'un ensemble de résultats. Ceci permet aux lots [!INCLUDE[tsql](../../includes/tsql-md.md)], aux procédures stockées et aux déclencheurs de travailler sur une seule ligne de résultat à la fois. Cela signifie aussi que la procédure ne peut pas être appelée directement à partir des fonctions d'API de base de données. Le paramètre de sortie de curseur doit être lié à une variable de programme, mais les API de base de données ne prennent pas en charge les paramètres ou les variables de curseur de liaison.  
  
 Ceci est le format du curseur renvoyé par sp_cursor_list. Le format du curseur est identique à celui qui est renvoyé par sp_describe_cursor.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|reference_name|**sysname**|Le nom utilisé pour faire référence au curseur. Si la référence au curseur était faite via le nom donné dans une instruction DECLARE CURSOR, le nom de référence est le même que le nom du curseur. Si la référence au curseur était faite via une variable, le nom de référence est le même que le nom de la variable de curseur.|  
|cursor_name|**sysname**|Nom du curseur dans une instruction DECLARE CURSOR. Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], si le curseur a été créé en définissant une variable de curseur pour un curseur, **cursor_name** retourne le nom de la variable de curseur.  Dans les versions précédentes, cette colonne de sortie retourne un nom généré par le système.|  
|cursor_scope|**smallint**|1 = LOCAL<br /><br /> 2 = GLOBAL|  
|status|**smallint**|Valeurs identiques à celles indiquées par la fonction système CURSOR_STATUS :<br /><br /> 1 = Le curseur référencé par le nom de curseur ou la variable est ouvert. Si le curseur est non sensitif, statique ou contrôlé par clés, il comporte au moins une ligne. Si le curseur est dynamique, l'ensemble de résultats comporte zéro ou plusieurs lignes.<br /><br /> 0 = Le curseur référencé par le nom de curseur ou la variable est ouvert mais ne comporte pas de lignes. Les curseurs dynamiques ne renvoient jamais cette valeur.<br /><br /> -1 = Le curseur référencé par le nom de curseur ou la variable est fermé.<br /><br /> -2 = S'applique uniquement aux variables de curseur. Aucun curseur n'est affecté à la variable. Il se peut qu'un paramètre OUTPUT ait affecté un curseur à la variable, mais la procédure stockée a fermé le curseur avant de sortir.<br /><br /> -3 = Aucun curseur ou variable de curseur portant le nom spécifié n'existe, ou aucun curseur n'a été alloué à la variable de curseur.|  
|model|**smallint**|1 = Non sensitif (ou statique)<br /><br /> 2 = jeu de clés<br /><br /> 3 = dynamique<br /><br /> 4 = Avance rapide|  
|concurrence|**smallint**|1 = lecture seule<br /><br /> 2 = Verrous de défilement<br /><br /> 3 = Optimiste|  
|scrollable|**smallint**|0 = Avant uniquement<br /><br /> 1 = À défilement|  
|open_status|**smallint**|0 = Fermé<br /><br /> 1 = Ouvert|  
|cursor_rows|**int**|Nombre de lignes éligibles dans l'ensemble de résultats. Pour plus d’informations, consultez [@@CURSOR_ROWS](../../t-sql/functions/cursor-rows-transact-sql.md).|  
|fetch_status|**smallint**|État de la dernière opération d'extraction sur ce curseur. Pour plus d’informations, consultez [@@FETCH_STATUS](../../t-sql/functions/fetch-status-transact-sql.md):<br /><br /> 0 = Opération d'extraction réussie.<br /><br /> -1 = L'opération d'extraction a échoué ou est hors des limites du curseur.<br /><br /> -2 = La ligne demandée est manquante.<br /><br /> -9 = Il n'y a pas eu d'opération d'extraction sur le curseur.|  
|column_count|**smallint**|Nombre de colonnes dans l'ensemble de résultats du curseur.|  
|row_count|**smallint**|Le nombre de lignes affectées par la dernière opération sur le curseur. Pour plus d’informations, consultez [@@ROWCOUNT](../../t-sql/functions/rowcount-transact-sql.md).|  
|last_operation|**smallint**|Dernière opération effectuée sur le curseur :<br /><br /> 0 = Aucune opération n'a été effectuée sur le curseur.<br /><br /> 1 = OPEN<br /><br /> 2 = FETCH<br /><br /> 3 = INSERTION<br /><br /> 4 = UPDATE<br /><br /> 5 = SUPPRESSION<br /><br /> 6 = CLOSE<br /><br /> 7 = DEALLOCATE|  
|cursor_handle|**int**|Valeur unique identifiant le curseur dans l'étendue du serveur.|  
  
## <a name="remarks"></a>Notes  
 sp_cursor_list génère la liste des curseurs de serveur actifs ouverts par la connexion et décrit les attributs globaux de chaque curseur, notamment la possibilité de les faire défiler et de les mettre à jour. Les curseurs répertoriés par sp_cursor_list sont les suivants :  
  
-   curseurs côté serveur [!INCLUDE[tsql](../../includes/tsql-md.md)] ;  
  
-   Curseurs de serveur API ouverts par une application ODBC qui appelle ensuite SQLSetCursorName pour nommer le curseur.  
  
 Utilisez sp_describe_cursor_columns pour obtenir une description des attributs du jeu de résultats retourné par le curseur. Utilisez sp_describe_cursor_tables pour générer un rapport sur les tables de base référencées par le curseur. sp_describe_cursor rapporte les mêmes informations que sp_cursor_list, mais uniquement pour un curseur spécifié.  
  
## <a name="permissions"></a>Autorisations  
 Les autorisations d'exécution sont attribuées par défaut au rôle public.  
  
## <a name="examples"></a>Exemples  
 Cet exemple ouvre un curseur global et utilise `sp_cursor_list` pour fournir un rapport sur les attributs du curseur.  
  
```  
USE AdventureWorks2012;  
GO  
-- Declare and open a keyset-driven cursor.  
DECLARE abc CURSOR KEYSET FOR  
SELECT LastName  
FROM Person.Person  
WHERE LastName LIKE 'S%';  
OPEN abc;  
  
-- Declare a cursor variable to hold the cursor output variable  
-- from sp_cursor_list.  
DECLARE @Report CURSOR;  
  
-- Execute sp_cursor_list into the cursor variable.  
EXEC master.dbo.sp_cursor_list @cursor_return = @Report OUTPUT,  
      @cursor_scope = 2;  
  
-- Fetch all the rows from the sp_cursor_list output cursor.  
FETCH NEXT from @Report;  
WHILE (@@FETCH_STATUS <> -1)  
BEGIN  
   FETCH NEXT from @Report;  
END  
  
-- Close and deallocate the cursor from sp_cursor_list.  
CLOSE @Report;  
DEALLOCATE @Report;  
GO  
  
-- Close and deallocate the original cursor.  
CLOSE abc;  
DEALLOCATE abc;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
