---
title: SET CONCAT_NULL_YIELDS_NULL (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-data-warehouse, pdw, sql-database
ms.component: t-sql|statements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- CONCAT_NULL_YIELDS_NULL_TSQL
- SET CONCAT_NULL_YIELDS_NULL
- CONCAT_NULL_YIELDS_NULL
- SET_CONCAT_NULL_YIELDS_NULL_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CONCAT_NULL_YIELDS_NULL option
- null values [SQL Server], concatenation results
- concatenation [SQL Server]
- SET CONCAT_NULL_YIELDS_NULL statement
ms.assetid: 3091b71c-6518-4eb4-88ab-acae49102bc5
caps.latest.revision: 34
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: cc4261d54d86300783499871f50b5ec7d7d152fd
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="set-concatnullyieldsnull-transact-sql"></a>SET CONCAT_NULL_YIELDS_NULL (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-asdw-pdw-md](../../includes/tsql-appliesto-ss2008-xxxx-asdw-pdw-md.md)]

  Détermine si les résultats de concaténation sont considérés comme des valeurs nulles ou des chaînes vides.  
  
> [!IMPORTANT]  
>  Dans une future version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] CONCAT_NULL_YIELDS_NULL sera toujours ON et toute application qui définira explicitement l'option à OFF générera une erreur. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
-- Syntax for SQL Server  
    
SET CONCAT_NULL_YIELDS_NULL { ON | OFF }   
```  
  
```  
-- Syntax for Azure SQL Data Warehouse and Parallel Data Warehouse  
  
SET CONCAT_NULL_YIELDS_NULL ON    
```  
  
## <a name="remarks"></a>Notes   
 Si SET CONCAT_NULL_YIELDS_NULL a la valeur ON, la concaténation d'une valeur NULL avec une chaîne retourne un résultat NULL. Par exemple, `SELECT 'abc' + NULL` donne `NULL`. Si SET CONCAT_NULL_YIELDS_NULL a la valeur OFF, la concaténation d'une valeur NULL avec une chaîne renvoie la chaîne ; la valeur NULL est considérée comme une chaîne vide. Par exemple, `SELECT 'abc' + NULL` donne `abc`.  
  
 Si SET CONCAT_NULL_YIELDS_NULL n’est pas spécifié, le paramètre de l’option de base de données **CONCAT_NULL_YIELDS_NULL** s’applique.  
  
> [!NOTE]  
>  SET CONCAT_NULL_YIELDS_NULL équivaut au paramètre CONCAT_NULL_YIELDS_NULL de ALTER DATABASE.  
  
 L'option SET CONCAT_NULL_YIELDS_NULL est définie lors de l'exécution, et non pas durant l'analyse.  
  
 SET CONCAT_NULL_YIELDS_NULL doit avoir la valeur ON lors de la création ou de la modification d'index dans des colonnes calculées ou des vues indexées. Si SET CONCAT_NULL_YIELDS_NULL a la valeur OFF, toute instruction CREATE, UPDATE, INSERT ou DELETE dans des tables comportant des index de colonnes calculées ou de vues indexées échoue. Pour plus d’informations sur les paramètres de l’option SET obligatoire avec les vues indexées et les index sur des colonnes calculées, consultez « Remarques sur l’utilisation des instructions SET » dans [Instructions SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md).  
  
 Lorsque CONCAT_NULL_YIELDS_NULL a la valeur OFF, la concaténation de chaîne est impossible au-delà des limites du serveur.  
  
 Pour afficher la valeur actuelle de ce paramètre, exécutez la requête suivante.  
  
```  
DECLARE @CONCAT_NULL_YIELDS_NULL VARCHAR(3) = 'OFF';  
IF ( (4096 & @@OPTIONS) = 4096 ) SET @CONCAT_NULL_YIELDS_NULL = 'ON';  
SELECT @CONCAT_NULL_YIELDS_NULL AS CONCAT_NULL_YIELDS_NULL;  
  
```  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant illustre l'utilisation des deux paramètres `SET CONCAT_NULL_YIELDS_NULL`.  
  
```  
PRINT 'Setting CONCAT_NULL_YIELDS_NULL ON';  
GO  
-- SET CONCAT_NULL_YIELDS_NULL ON and testing.  
SET CONCAT_NULL_YIELDS_NULL ON;  
GO  
SELECT 'abc' + NULL ;  
GO  
  
-- SET CONCAT_NULL_YIELDS_NULL OFF and testing.  
SET CONCAT_NULL_YIELDS_NULL OFF;  
GO  
SELECT 'abc' + NULL;   
GO  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Instructions SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)   
 [SESSIONPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/sessionproperty-transact-sql.md)  
  
  
