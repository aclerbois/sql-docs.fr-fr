---
title: SET STATISTICS PROFILE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: t-sql|statements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- PROFILE
- SET_STATISTICS_PROFILE_TSQL
- PROFILE_TSQL
- SET STATISTICS PROFILE
dev_langs:
- TSQL
helpviewer_keywords:
- profiles [SQL Server], displaying
- statements [SQL Server], profile information
- SET STATISTICS PROFILE statement
- STATISTICS PROFILE option
- statistical information [SQL Server], profiles
ms.assetid: c635e262-35fa-421a-aa6f-a1c30f351647
caps.latest.revision: 34
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 6494f792c1dcfbb92bfe48d3063df533d0ff1510
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="set-statistics-profile-transact-sql"></a>SET STATISTICS PROFILE (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Affiche des informations sur le profil d'une instruction. L'option STATISTICS PROFILE peut être utilisée avec des requêtes ad hoc, des vues et des procédures stockées.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
SET STATISTICS PROFILE { ON | OFF }  
```  
  
## <a name="remarks"></a>Notes   
 Si STATISTICS PROFILE est défini sur ON, chaque requête exécutée renvoie son jeu de résultats habituel, suivi d'un second jeu de résultats affichant un profil de l'exécution de la requête.  
  
 Le second jeu de résultats contient les colonnes SHOWPLAN_ALL se rapportant à la requête, ainsi que les colonnes supplémentaires suivantes :  
  
|Nom de colonne|Description|  
|-----------------|-----------------|  
|**Lignes**|Nombre réel de lignes générées par chaque opérateur.|  
|**Exécutions**|Nombre de fois où l'opérateur s'est exécuté.|  
  
## <a name="permissions"></a>Autorisations  
 Pour utiliser SET STATISTICS PROFILE et afficher les résultats, les utilisateurs doivent disposer des autorisations suivantes :  
  
-   Autorisations adéquates pour exécuter les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
-   Autorisation SHOWPLAN sur toutes les bases de données contenant des objets auxquels les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] font référence.  
  
 En cas d'instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] ne générant aucun jeu de résultats STATISTICS PROFILE, seules les autorisations adéquates pour exécuter les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] s'avèrent nécessaires. En cas d'instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] ne générant pas de jeu de résultats STATISTICS PROFILE, les autorisations d'exécution d'instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] et SHOWPLAN doivent être vérifiées et validées, ou l'exécution des instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] est abandonnée et aucune information de représentation n'est générée.  
  
## <a name="see-also"></a> Voir aussi  
 [Instructions SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)   
 [SET SHOWPLAN_ALL &#40;Transact-SQL&#41;](../../t-sql/statements/set-showplan-all-transact-sql.md)   
 [SET STATISTICS TIME &#40;Transact-SQL&#41;](../../t-sql/statements/set-statistics-time-transact-sql.md)   
 [SET STATISTICS IO &#40;Transact-SQL&#41;](../../t-sql/statements/set-statistics-io-transact-sql.md)  
  
  
