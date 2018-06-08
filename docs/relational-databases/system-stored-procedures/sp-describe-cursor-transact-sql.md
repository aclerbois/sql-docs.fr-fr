---
title: sp_describe_cursor (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_describe_cursor
- sp_describe_cursor_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_describe_cursor
ms.assetid: 0c836c99-1147-441e-998c-f0a30cd05275
caps.latest.revision: 22
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 72278631cebc617666317df77fd62e28442b9706
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spdescribecursor-transact-sql"></a>sp_describe_cursor (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Indique les attributs d'un curseur côté serveur.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_describe_cursor [ @cursor_return = ] output_cursor_variable OUTPUT   
     { [ , [ @cursor_source = ] N'local'  
    , [ @cursor_identity = ] N'local_cursor_name' ]   
   | [ , [ @cursor_source = ] N'global'  
    , [ @cursor_identity = ] N'global_cursor_name' ]   
   | [ , [ @cursor_source = ] N'variable'  
     , [ @cursor_identity = ] N'input_cursor_variable' ]   
     }   
[;]  
```  
  
## <a name="arguments"></a>Arguments  
 [ @cursor_return=] *variable_de_curseur_sortie* sortie  
 Nom d'une variable de curseur déclarée devant recevoir la sortie du curseur. *output_cursor_variable* est **curseur**, sans valeur par défaut et doivent ne pas être associé à des curseurs au moment de l’appel de sp_describe_cursor. Le curseur retourné est un curseur en lecture seule, dynamique et permettant les défilements.  
  
 [ @cursor_source=] {Ne local ' | Ne global ' | Ne variable '}  
 Indique si le curseur qui fait l'objet du rapport est défini en utilisant le nom d'un curseur local, d'un curseur global ou d'une variable de curseur. Le paramètre est **nvarchar (30)**.  
  
 [ @cursor_identity=] N'*nom_de_curseur_local*']  
 Nom d'un curseur créé par une instruction DECLARE CURSOR contenant soit le mot clé LOCAL, soit celui défini par défaut pour LOCAL. *nom_de_curseur_local* est **nvarchar (128)**.  
  
 [ @cursor_identity=] N'*ne nom_de_curseur_global*']  
 Est le nom d’un curseur créé par une instruction DECLARE CURSOR contenant soit le mot clé GLOBAL, soit celui défini par défaut pour GLOBAL. *ne nom_de_curseur_global* est **nvarchar (128)**.  
  
 *ne nom_de_curseur_global* peut également être le nom d’un curseur de serveur API ouvert par une application ODBC qui a ensuite nommé en appelant SQLSetCursorName.  
  
 [ @cursor_identity=] N'*ne variable_de_curseur_entrée*']  
 Nom d'une variable de curseur associée à un curseur ouvert. *Ne variable_de_curseur_entrée* est **nvarchar (128)**.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 Aucun  
  
## <a name="cursors-returned"></a>Curseurs retournés  
 sp_describe_cursor encapsule son jeu de résultats dans un [!INCLUDE[tsql](../../includes/tsql-md.md)] **curseur** paramètre de sortie. Cela permet aux lots, procédures stockées et déclencheurs [!INCLUDE[tsql](../../includes/tsql-md.md)] de travailler sur une seule ligne de sortie à la fois. Cela signifie également que la procédure ne peut pas être appelée directement à partir des fonctions d’API de base de données. Le **curseur** paramètre de sortie doit être lié à une variable de programme, mais l’API de bases de données ne prennent pas en charge liaison **curseur** variables ou des paramètres.  
  
 La table suivante indique le format du curseur qui est retourné en utilisant sp_describe_cursor. C'est le même format que celui qui est retourné par sp_cursor_list.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|reference_name|**sysname**|Nom utilisé pour désigner le curseur. Si la référence du curseur provient du nom spécifié dans une instruction DECLARE CURSOR, le nom de référence est le même que le nom du curseur. Si la référence du curseur provient d'une variable, le nom de référence est celui de la variable.|  
|cursor_name|**sysname**|Nom du curseur dans une instruction DECLARE CURSOR. Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], si le curseur a été créé en définissant une variable curseur pour un curseur, cursor_name retourne le nom de la variable curseur. Dans les versions précédentes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], cette colonne de résultat retourne un nom généré par le système.|  
|cursor_scope|**tinyint**|1 = LOCAL<br /><br /> 2 = GLOBAL|  
|status|**int**|Valeurs identiques à celles indiquées par la fonction système CURSOR_STATUS :<br /><br /> 1 = Le curseur référencé par le nom de curseur ou la variable est ouvert. Si le curseur est non sensitif, statique ou contrôlé par clés, il comporte au moins une ligne. Si le curseur est dynamique, l'ensemble de résultats comporte zéro ou plusieurs lignes.<br /><br /> 0 = Le curseur référencé par le nom de curseur ou la variable est ouvert mais ne comporte pas de lignes. Les curseurs dynamiques ne renvoient jamais cette valeur.<br /><br /> -1 = Le curseur référencé par le nom de curseur ou la variable est fermé.<br /><br /> -2 = S'applique uniquement aux variables de curseur. Aucun curseur n'est affecté à la variable. Il se peut qu'un paramètre OUTPUT ait affecté un curseur à la variable, mais la procédure stockée a fermé le curseur avant de sortir.<br /><br /> -3 = Aucun curseur ou variable de curseur portant le nom spécifié n'existe, ou aucun curseur n'a été alloué à la variable de curseur.|  
|model|**tinyint**|1 = Non sensitif (ou statique)<br /><br /> 2 = jeu de clés<br /><br /> 3 = dynamique<br /><br /> 4 = Avance rapide|  
|concurrence|**tinyint**|1 = lecture seule<br /><br /> 2 = Verrous de défilement<br /><br /> 3 = Optimiste|  
|scrollable|**tinyint**|0 = Avant uniquement<br /><br /> 1 = À défilement|  
|open_status|**tinyint**|0 = Fermé<br /><br /> 1 = Ouvert|  
|cursor_rows|**Decimal(10,0)**|Nombre de lignes correspondantes dans le jeu de résultats. Pour plus d’informations, consultez [@@CURSOR_ROWS &#40;Transact-SQL&#41;](../../t-sql/functions/cursor-rows-transact-sql.md).|  
|fetch_status|**smallint**|État de la dernière extraction sur ce curseur. Pour plus d’informations, consultez [@@FETCH_STATUS &#40;Transact-SQL&#41;](../../t-sql/functions/fetch-status-transact-sql.md).<br /><br /> 0 = Opération d'extraction réussie.<br /><br /> -1 = L'opération d'extraction a échoué ou est hors des limites du curseur.<br /><br /> -2 = La ligne demandée est manquante.<br /><br /> -9 = Il n'y a pas eu d'opération d'extraction sur le curseur.|  
|column_count|**smallint**|Nombre de colonnes dans le jeu de résultats du curseur|  
|row_count|**Decimal(10,0)**|Nombre de lignes affectées par la dernière opération sur le curseur. Pour plus d’informations, consultez [@@ROWCOUNT &#40;Transact-SQL&#41;](../../t-sql/functions/rowcount-transact-sql.md).|  
|last_operation|**tinyint**|Dernière opération effectuée sur le curseur :<br /><br /> 0 = Aucune opération n'a été effectuée sur le curseur.<br /><br /> 1 = OPEN<br /><br /> 2 = FETCH<br /><br /> 3 = INSERTION<br /><br /> 4 = UPDATE<br /><br /> 5 = SUPPRESSION<br /><br /> 6 = CLOSE<br /><br /> 7 = DEALLOCATE|  
|cursor_handle|**int**|Valeur unique pour le curseur dans l'étendue du serveur.|  
  
## <a name="remarks"></a>Notes  
 sp_describe_cursor décrit les attributs globaux d'un curseur de serveur, comme la possibilité de défiler ou d'être mis à jour. Utilisez sp_describe_cursor_columns pour obtenir une description des attributs du jeu de résultats retourné par le curseur. Utilisez sp_describe_cursor_tables pour générer un rapport sur les tables de base référencées par le curseur. Pour obtenir un rapport sur les curseurs côté serveur [!INCLUDE[tsql](../../includes/tsql-md.md)] visibles pour la connexion, utilisez sp_cursor_list.  
  
 Une instruction DECLARE CURSOR peut demander un type de curseur que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne peut pas prendre en charge en utilisant l'instruction SELECT contenue dans DECLARE CURSOR. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] convertit de manière implicite le curseur en un type qu'il peut prendre en charge en utilisant l'instruction SELECT. Si l'option TYPE_WARNING est spécifiée dans l'instruction DECLARE CURSOR, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] envoie à l'application un message d'information lui indiquant qu'une conversion a été effectuée. sp_describe_cursor peut ensuite être appelée pour déterminer le type de curseur qui a été implémenté.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle public.  
  
## <a name="examples"></a>Exemples  
 Cet exemple ouvre un curseur global et utilise `sp_describe_cursor` pour fournir un rapport sur les attributs du curseur.  
  
```  
USE AdventureWorks2012;  
GO  
-- Declare and open a global cursor.  
DECLARE abc CURSOR STATIC FOR  
SELECT LastName  
FROM Person.Person;  
  
OPEN abc;  
  
-- Declare a cursor variable to hold the cursor output variable  
-- from sp_describe_cursor.  
DECLARE @Report CURSOR;  
  
-- Execute sp_describe_cursor into the cursor variable.  
EXEC master.dbo.sp_describe_cursor @cursor_return = @Report OUTPUT,  
        @cursor_source = N'global', @cursor_identity = N'abc';  
  
-- Fetch all the rows from the sp_describe_cursor output cursor.  
FETCH NEXT from @Report;  
WHILE (@@FETCH_STATUS <> -1)  
BEGIN  
    FETCH NEXT from @Report;  
END  
  
-- Close and deallocate the cursor from sp_describe_cursor.  
CLOSE @Report;  
DEALLOCATE @Report;  
GO  
  
-- Close and deallocate the original cursor.  
CLOSE abc;  
DEALLOCATE abc;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Curseurs](../../relational-databases/cursors.md)   
 [CURSOR_STATUS &#40;Transact-SQL&#41;](../../t-sql/functions/cursor-status-transact-sql.md)   
 [DECLARE CURSOR &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-cursor-transact-sql.md)   
 [sp_cursor_list &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-cursor-list-transact-sql.md)   
 [sp_describe_cursor_columns &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-describe-cursor-columns-transact-sql.md)   
 [sp_describe_cursor_tables &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-describe-cursor-tables-transact-sql.md)  
  
  
