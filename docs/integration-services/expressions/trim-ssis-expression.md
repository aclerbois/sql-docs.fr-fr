---
title: TRIM (expression SSIS) | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: expressions
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- leading blanks
- TRIM function
- trailing blanks
ms.assetid: 7dd9081d-a3d4-483a-bf7e-bf2bd7692d39
caps.latest.revision: 34
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: b2333f7c0a74b664e8c772def787cbbd3dd8b71a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="trim-ssis-expression"></a>TRIM (expression SSIS)
  Renvoie une chaîne de type caractère après la suppression des espaces de début et de fin.  
  
> [!NOTE]  
>  La fonction TRIM ne supprime pas les espaces blancs tels que les caractères de tabulation ou de saut de ligne. Le codage Unicode founit des points de code pour divers types d'espaces, mais cette fonction ne reconnaît que le point de code Unicode 0x0020. Ainsi, lorsque les chaînes du jeu de caractères codés sur deux octets (DBCS) sont converties en Unicode, elles peuvent comporter d'autres caractères d'espaces que 0x0020 si bien que la fonction ne peut pas les supprimer. Pour éliminer tout type d'espace, utilisez par exemple la méthode de suppression des espaces (Trim) de Microsoft Visual Basic .NET dans un script exécuté à partir du composant Script.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
TRIM(character_expression)  
```  
  
## <a name="arguments"></a>Arguments  
 *expression_caractère*  
 Expression de caractères dont les espaces doivent être supprimés.  
  
## <a name="result-types"></a>Types des résultats  
 DT_WSTR  
  
## <a name="remarks"></a>Notes   
 La fonction TRIM renvoie un résultat NULL si l'argument est NULL.  
  
 La fonction TRIM fonctionne seulement avec le type de données DT_WSTR. Un argument *character_expression* qui est un littéral de chaîne ou une colonne de données avec le type de données DT_STR est implicitement converti dans le type de données DT_WSTR avant que TRIM effectue son opération. Les autres types de données doivent être explicitement convertis vers le type de données DT_WSTR. Pour plus d’informations, consultez [Types de données d’Integration Services](../../integration-services/data-flow/integration-services-data-types.md) et [Cast &#40;expression SSIS&#41;](../../integration-services/expressions/cast-ssis-expression.md).  
  
## <a name="expression-examples"></a>Exemples d'expressions  
 L'exemple suivant supprime les espaces de début et de fin d'un littéral de chaîne. Le résultat obtenu est «New York».  
  
```  
TRIM("   New York   ")  
```  
  
 L'exemple suivant supprime les espaces de début et de fin de la concaténation des colonnes **FirstName** et **LastName** . La chaîne vide entre les colonnes **FirstName** et **LastName** n'est pas supprimée.  
  
```  
TRIM(FirstName + " "+ LastName)  
```  
  
## <a name="see-also"></a> Voir aussi  
 [LTRIM &#40;expression SSIS&#41;](../../integration-services/expressions/ltrim-ssis-expression.md)   
 [RTRIM &#40;expression SSIS&#41;](../../integration-services/expressions/rtrim-ssis-expression.md)   
 [Fonctions &#40;expression SSIS&#41;](../../integration-services/expressions/functions-ssis-expression.md)  
  
  
