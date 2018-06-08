---
title: Fonctions XQuery impliquant le Type de données xml | Documents Microsoft
ms.custom: ''
ms.date: 03/09/2017
ms.prod: sql
ms.prod_service: sql
ms.component: xquery
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: language-reference
applies_to:
- SQL Server
dev_langs:
- XML
helpviewer_keywords:
- XQuery, functions
- xml data type [SQL Server], XQuery
- functions [SQL Server], XQuery
ms.assetid: 8df0877d-a03f-4ca9-b84e-908c4bb42b5e
caps.latest.revision: 37
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: db1906027692ec40974668f48521588b2231cebd
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="xquery-functions-against-the-xml-data-type"></a>Fonctions XQuery impliquant le type de données xml
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

  Cette rubrique et ses sous-rubriques décrivent les fonctions que vous pouvez utiliser lors de la spécification XQuery sur le **xml** type de données. Pour les spécifications W3C, consultez [ http://www.w3.org/TR/2004/WD-xpath-functions-20040723 ](http://go.microsoft.com/fwlink/?LinkId=4873).  
  
 Les fonctions XQuery appartiennent à la http://www.w3.org/2004/07/xpath-functions espace de noms. Les spécifications W3C utilisent le préfixe d'espace de noms « fn: » pour décrire ces fonctions. Vous n'avez pas besoin de spécifier ce préfixe explicitement lorsque vous utilisez les fonctions. Par conséquent, et dans un souci de lisibilité, les préfixes d'espace de noms ne sont généralement pas utilisés dans cette documentation.  
  
 Le tableau suivant répertorie les fonctions XQuery prises en charge par rapport à la **xml**type de données.  
  
|Catégorie|Nom de fonction|  
|--------------|-------------------|  
|[Fonctions sur des valeurs numériques](http://msdn.microsoft.com/library/d5740a32-b174-43b9-b64d-1cc6edc50cff)|[Plafond](../xquery/numeric-values-functions-ceiling.md)|  
||[Floor](../xquery/numeric-values-functions-floor.md)|  
||[Arrondir](../xquery/numeric-values-functions-round.md)|  
|[Fonctions XQuery sur des valeurs de chaîne](http://msdn.microsoft.com/library/2dccefef-5d90-4f56-bda7-4c1954d8a730)|[concat](../xquery/functions-on-string-values-concat.md)|  
||[contient](../xquery/functions-on-string-values-contains.md)|  
||[sous-chaîne](../xquery/functions-on-string-values-substring.md)|  
||[Fonction LOWER-case &#40;XQuery&#41;](../xquery/functions-on-string-values-lower-case.md)|  
||[longueur de chaîne](../xquery/functions-on-string-values-string-length.md)|  
||[Fonction UPPER-case &#40;XQuery&#41;](../xquery/functions-on-string-values-upper-case.md)|  
|Fonctions sur des valeurs booléennes|[pas](../xquery/functions-on-boolean-values-not-function.md)|  
|[Fonctions sur les nœuds](http://msdn.microsoft.com/library/09a8affa-3341-4f50-aebc-fdf529e00c08)|[number](../xquery/functions-on-nodes-number.md)|  
||[Fonction de local-name (XQuery)](../xquery/functions-on-nodes-local-name.md)|  
||[uri de l’espace de noms, fonction (XQuery)](../xquery/functions-on-nodes-namespace-uri.md)|  
|[Fonctions relatives au contexte](http://msdn.microsoft.com/library/f7d8af33-9de9-450c-a667-23dee3129b5f)|[last](../xquery/context-functions-last-xquery.md)|  
||[position](../xquery/context-functions-position-xquery.md)|  
|[Fonctions sur les séquences](http://msdn.microsoft.com/library/672d2795-53ab-49c2-bf24-bc81a47ecd3f)|[vide](../xquery/functions-on-sequences-empty.md)|  
||[valeurs distinctes](../xquery/functions-on-sequences-distinct-values.md)|  
||[ID de fonction (XQuery)](../xquery/functions-on-sequences-id.md)|  
|[Fonctions d’agrégation &#40;XQuery&#41;](http://msdn.microsoft.com/library/be647ef1-291e-4a5d-ab18-07c759efe176)|[nombre](../xquery/aggregate-functions-count.md)|  
||[AVG](../xquery/aggregate-functions-avg.md)|  
||[min](../xquery/aggregate-functions-min.md)|  
||[max](../xquery/aggregate-functions-max.md)|  
||[Somme](../xquery/aggregate-functions-sum.md)|  
|[Fonctions constructeur &#40;XQuery&#41;](../xquery/constructor-functions-xquery.md)|[Fonctions constructeur](../xquery/constructor-functions-xquery.md)|  
|[Fonctions d’accesseurs de données](../xquery/data-accessor-functions.md)|[chaîne](../xquery/data-accessor-functions-string-xquery.md)|  
||[data](../xquery/data-accessor-functions-data-xquery.md)|  
|[Fonctions de constructeur booléennes &#40;XQuery&#41;](http://msdn.microsoft.com/library/fa907f39-d4b7-4495-b829-c788928e0f64)|[Fonction True (XQuery)](../xquery/boolean-constructor-functions-true-xquery.md)|  
||[Fonction False (XQuery)](../xquery/boolean-constructor-functions-false-xquery.md)|  
|[Fonctions relatives aux QName &#40;XQuery&#41;](http://msdn.microsoft.com/library/7e07eb26-f551-4b63-ab77-861684faff71)|[Expanded-QName (XQuery)](../xquery/functions-related-to-qnames-expanded-qname.md)|  
||[local-nom-de-QName (XQuery)](../xquery/functions-related-to-qnames-local-name-from-qname.md)|  
||[espace de noms uri à partir de QName (XQuery)](../xquery/functions-related-to-qnames-namespace-uri-from-qname.md)|  
|[Fonctions d’Extension XQuery SQL Server](http://msdn.microsoft.com/library/4bc5d499-5fec-4c3f-b11e-5ab5ef9d8f97)|[fonction SQL :Column() (XQuery)](../xquery/xquery-extension-functions-sql-column.md)|  
||[fonction SQL :variable() (XQuery)](../xquery/xquery-extension-functions-sql-variable.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [méthodes de type de données xml](../t-sql/xml/xml-data-type-methods.md)   
 [Références relatives au langage Xquery &#40;SQL Server&#41;](../xquery/xquery-language-reference-sql-server.md)   
 [Données XML &#40;SQL Server&#41;](../relational-databases/xml/xml-data-sql-server.md)  
  
  
