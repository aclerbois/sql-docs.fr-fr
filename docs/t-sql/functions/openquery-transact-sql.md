---
title: OPENQUERY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: sql-database
ms.component: t-sql|functions
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- OPENQUERY_TSQL
- OPENQUERY
dev_langs:
- TSQL
helpviewer_keywords:
- DELETE statement [SQL Server], OPENQUERY function
- OPENQUERY function
- FROM clause, OPENQUERY function
- UPDATE statement [SQL Server], OPENQUERY function
- pass-through queries [SQL Server]
- INSERT statement [SQL Server], OPENQUERY function
ms.assetid: b805e976-f025-4be1-bcb0-3a57b0c57717
caps.latest.revision: 42
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: c12b060ae420272a3e454dff118ca828c144fe63
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="openquery-transact-sql"></a>OPENQUERY (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Exécute la requête directe spécifiée sur le serveur lié spécifié. Ce serveur est une source de données OLE DB. Il est possible de référencer OPENQUERY dans la clause FROM d’une requête SELECT comme s’il s’agissait du nom d’une table. Il est également possible de référencer OPENQUERY comme la table cible d'une instruction INSERT, UPDATE ou DELETE. Cela dépend des possibilités du fournisseur OLE DB. Bien que la requête puisse renvoyer plusieurs jeux de résultats, OPENQUERY ne renvoie que le premier.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
OPENQUERY ( linked_server ,'query' )  
```  
  
## <a name="arguments"></a>Arguments  
 *linked_server*  
 ID représentant le nom du serveur lié.  
  
 **'** *query* **'**  
 Chaîne de requête exécutée dans le serveur lié. La longueur maximale de la chaîne est limitée à 8 Ko.  
  
## <a name="remarks"></a>Notes   
 OPENQUERY n'accepte pas de variables pour ses arguments.  
  
 Vous ne pouvez pas utiliser OPENQUERY pour exécuter des procédures stockées étendues sur un serveur lié. Par contre, une procédure stockée étendue peut être exécutée sur un serveur lié en utilisant un nom en quatre parties. Exemple :  
  
```sql  
EXEC SeattleSales.master.dbo.xp_msver  
```  
  
 Tout appel à OPENDATASOURCE, OPENQUERY ou OPENROWSET dans la clause FROM est évalué séparément et indépendamment de tout appel à ces fonctions utilisé comme cible de la mise à jour, même si des arguments identiques sont fournis aux deux appels. En particulier, les conditions de filtre ou de jointure appliquées sur le résultat de l'un de ces appels n'ont aucun effet sur les résultats de l'autre.  
  
## <a name="permissions"></a>Autorisations  
 Tous les utilisateurs peuvent exécuter OPENQUERY. Les autorisations utilisées pour la connexion au serveur distant sont obtenues à partir des paramètres définis pour le serveur lié.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-executing-an-update-pass-through-query"></a>A. Exécution d'une requête UPDATE directe  
 L'exemple suivant exécute une requête `UPDATE` directe sur le serveur lié créé dans l'exemple A.  
  
```sql  
UPDATE OPENQUERY (OracleSvr, 'SELECT name FROM joe.titles WHERE id = 101')   
SET name = 'ADifferentName';  
```  
  
### <a name="b-executing-an-insert-pass-through-query"></a>B. Exécution d'une requête INSERT directe  
 L'exemple suivant exécute une requête `INSERT` directe sur le serveur lié créé dans l'exemple A.  
  
```sql  
INSERT OPENQUERY (OracleSvr, 'SELECT name FROM joe.titles')  
VALUES ('NewTitle');  
```  
  
### <a name="c-executing-a-delete-pass-through-query"></a>C. Exécution d'une requête DELETE directe  
 L'exemple suivant exécute une requête `DELETE` directe pour supprimer la ligne insérée dans l'exemple C.  
  
```sql  
DELETE OPENQUERY (OracleSvr, 'SELECT name FROM joe.titles WHERE name = ''NewTitle''');  
```  
  
### <a name="d-executing-a-select-pass-through-query"></a>D. Exécution d'une requête SELECT directe  
 L’exemple suivant exécute une requête directe `SELECT` pour sélectionner la ligne insérée dans l’exemple C.  
  
```sql  
SELECT * FROM OPENQUERY (OracleSvr, 'SELECT name FROM joe.titles WHERE name = ''NewTitle''');  
```  
    
## <a name="see-also"></a> Voir aussi  
 [DELETE &#40;Transact-SQL&#41;](../../t-sql/statements/delete-transact-sql.md)   
 [FROM &#40;Transact-SQL&#41;](../../t-sql/queries/from-transact-sql.md)   
 [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)   
 [OPENDATASOURCE &#40;Transact-SQL&#41;](../../t-sql/functions/opendatasource-transact-sql.md)   
 [OPENROWSET &#40;Transact-SQL&#41;](../../t-sql/functions/openrowset-transact-sql.md)   
 [Fonctions d’ensemble de lignes &#40;Transact-SQL&#41;](../../t-sql/functions/rowset-functions-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [sp_addlinkedserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md)   
 [sp_serveroption &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-serveroption-transact-sql.md)   
 [UPDATE &#40;Transact-SQL&#41;](../../t-sql/queries/update-transact-sql.md)   
 [WHERE &#40;Transact-SQL&#41;](../../t-sql/queries/where-transact-sql.md)  
  
  
