---
title: SET FIPS_FLAGGER (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: t-sql|statements
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- FIPS_FLAGGER
- SET_FIPS_FLAGGER_TSQL
- FIPS_FLAGGER_TSQL
- SET FIPS_FLAGGER
dev_langs:
- TSQL
helpviewer_keywords:
- SET FIPS_FLAGGER statement
- FIPS 127-2 standard
- FIPS_FLAGGER option
ms.assetid: e82f6bee-6cf6-4061-be22-9ad2e8e9d3d6
caps.latest.revision: 31
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 8d52cc3059bd1c0497511b9b230c5f2d57eef8c3
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="set-fipsflagger-transact-sql"></a>SET FIPS_FLAGGER (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Spécifie la vérification de conformité à la norme FIPS 127-2. Ce contrôle est basé sur la norme ISO. Pour plus d’informations sur la conformité aux normes FIPS SQL Server, consultez [How to use SQL Server 2016 in FIPS 140-2-compliant mode (Guide pratique pour utiliser SQL Server 2016 en mode compatible avec FIPS 140-2)](https://support.microsoft.com/help/4014354/how-to-use-sql-server-2016-in-fips-140-2-compliant-mode). 
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
SET FIPS_FLAGGER ( 'level' |  OFF )  
```  
  
## <a name="arguments"></a>Arguments  
 **'** *level* **'**  
 Niveau de conformité à la norme FIPS 127-2, vérifié dans toutes les opérations effectuées dans les bases de données. Si une opération de base de données entre en conflit avec le niveau des normes ISO choisi, [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] génère un avertissement.  
  
 *level* doit avoir l’une des valeurs suivantes.  
  
|Valeur|Description|  
|-----------|-----------------|  
|ENTRY|Vérification des normes pour la conformité ISO de base.|  
|FULL|Vérification des normes pour la conformité ISO complète.|  
|INTERMEDIATE|Vérification des normes pour la conformité ISO de niveau intermédiaire.|  
|OFF|Pas de vérification des normes.|  
  
## <a name="remarks"></a>Notes   
 L’option `SET FIPS_FLAGGER` est appliquée lors de l’analyse, et non lors de l’exécution. Par conséquent, si l’instruction SET est présente dans la procédure stockée ou le traitement d’instructions, elle devient effective, que l’exécution du code ait réellement atteint ou non ce point ; l’instruction `SET` devient effective avant l’exécution de toute autre instruction. Par exemple, même si l’instruction `SET` se trouve dans un bloc d’instructions `IF...ELSE` qui n’est jamais atteint pendant l’exécution, elle entre quand même en vigueur car le bloc d’instructions `IF...ELSE` est analysé.  
  
 Si l’option `SET FIPS_FLAGGER` est définie dans une procédure stockée, sa valeur est rétablie une fois que le contrôle est renvoyé par la procédure stockée. Par conséquent, une instruction dynamique `SET FIPS_FLAGGER` n’a aucun effet sur les instructions exécutées après celle-ci.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle **public** .  
  
## <a name="see-also"></a> Voir aussi  
 [Instructions SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)  
  
  
