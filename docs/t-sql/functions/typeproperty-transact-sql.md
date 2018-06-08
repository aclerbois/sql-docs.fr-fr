---
title: TYPEPROPERTY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: t-sql|functions
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- TYPEPROPERTY
- TYPEPROPERTY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- status information [SQL Server], data types
- data types [SQL Server], status information
- TYPEPROPERTY function
ms.assetid: bc311c80-bac5-46ab-a5c8-68b1c6bbf24a
caps.latest.revision: 43
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: e2da7509e05a6c66d86f7e14709612ae4e61c235
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="typeproperty-transact-sql"></a>TYPEPROPERTY (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-all-md](../../includes/tsql-appliesto-ss2008-all-md.md)]

  Renvoie des informations sur un type de données.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
TYPEPROPERTY (type , property)  
```  
  
## <a name="arguments"></a>Arguments  
 *type*  
 Nom du type de données.  
  
 *property*  
 Type d'informations à renvoyer pour le type de données. *property* peut avoir l’une des valeurs suivantes.  
  
|Propriété|Description|Valeur retournée|  
|--------------|-----------------|--------------------|  
|**AllowsNull**|Type de données autorisant les valeurs NULL.|1 = True<br /><br /> 0 = False<br /><br /> NULL = Type de données introuvable.|  
|**OwnerId**|Propriétaire du type.<br /><br /> Remarque : Le propriétaire du schéma n’est pas nécessairement le propriétaire du type.|Non NULL = ID utilisateur de base de données du propriétaire du type.<br /><br /> NULL = Type non pris en charge, ou ID de type non valide.|  
|**Précision**|Précision du type de données.|Nombre de chiffres ou de caractères.<br /><br /> -1 = **xml** ou type de données de valeur de grande taille<br /><br /> NULL = Type de données introuvable.|  
|**Échelle**|Échelle du type de données.|Nombre de décimales pour le type de données.<br /><br /> NULL = Le type de données n’est pas **numeric** ou est introuvable.|  
|**UsesAnsiTrim**|Le paramètre de remplissage ANSI était activé lors de la création du type de données.|1 = True<br /><br /> 0 = False<br /><br /> NULL = Type de données introuvable ou différent d'un type de données binaire ou chaîne.|  
  
## <a name="return-types"></a>Types de retour  
 **Int**  
  
## <a name="exceptions"></a>Exceptions  
 Retourne la valeur NULL en cas d'erreur ou si un appelant n'est pas autorisé à afficher l'objet.  
  
 Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], un utilisateur peut voir uniquement les métadonnées des éléments sécurisables qui lui appartiennent ou pour lesquels il dispose d'un droit d'accès. Cela signifie que les fonctions intégrées générant des métadonnées, telles que TYPEPROPERTY, peuvent retourner la valeur NULL si l'utilisateur ne dispose d'aucune autorisation sur l'objet. Pour plus d'informations, consultez [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-identifying-the-owner-of-a-data-type"></a>A. Identification du propriétaire d'un type de données  
 L'exemple suivant retourne le propriétaire d'un type de données.  
  
```  
SELECT TYPEPROPERTY(SCHEMA_NAME(schema_id) + '.' + name, 'OwnerId') AS owner_id, name, system_type_id, user_type_id, schema_id  
FROM sys.types;  
```  
  
### <a name="b-returning-the-precision-of-the-tinyint-data-type"></a>B. Renvoi de la précision du type de données tinyint  
 L'exemple suivant renvoie la précision ou le nombre de chiffres pour le type de données `tinyint`.  
  
```  
SELECT TYPEPROPERTY( 'tinyint', 'PRECISION');  
```  
  
## <a name="see-also"></a> Voir aussi  
 [TYPE_ID &#40;Transact-SQL&#41;](../../t-sql/functions/type-id-transact-sql.md)   
 [TYPE_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/type-name-transact-sql.md)   
 [COLUMNPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/columnproperty-transact-sql.md)   
 [Fonctions de métadonnées &#40;Transact-SQL&#41;](../../t-sql/functions/metadata-functions-transact-sql.md)   
 [OBJECTPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/objectproperty-transact-sql.md)   
 [ALTER AUTHORIZATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-authorization-transact-sql.md)   
 [sys.types &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-types-transact-sql.md)  
  
  

