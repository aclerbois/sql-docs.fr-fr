---
title: Utiliser la recherche en texte intégral avec des colonnes XML | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: xml
ms.reviewer: ''
ms.suite: sql
ms.technology: xml
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- xml columns [full-text search]
- indexes [full-text search]
ms.assetid: 8096cfc6-1836-4ed5-a769-a5d63b137171
caps.latest.revision: 14
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: c5d138753128b7a8447aa5671feeb388757791b7
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="use-full-text-search-with-xml-columns"></a>Utiliser la recherche en texte intégral avec des colonnes XML
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]
  Vous pouvez créer un index de texte intégral sur des colonnes XML de façon à indexer le contenu des valeurs XML tout en ignorant le balisage XML. Des balises d'éléments sont utilisées comme limites de jeton. Les éléments suivants sont indexés :  
  
-   Contenu des éléments XML.  
  
-   Contenu des attributs XML de l'élément de premier niveau uniquement, à moins qu'il ne s'agisse de valeurs numériques.  
  
 Lorsque cela est possible, vous pouvez associer une recherche en texte intégral et un index XML. Pour cela, procédez comme suit :  
  
1.  Tout d'abord, filtrez les valeurs XML pertinentes à l'aide d'une recherche en texte intégral SQL.  
  
2.  Ensuite, interrogez les valeurs XML pour lesquelles il existe un index XML sur la colonne XML.  
  
## <a name="example-combining-full-text-search-with-xml-querying"></a>Exemple : association d'une recherche en texte intégral avec une requête XML  
 Une fois l'index de texte intégral créé sur la colonne XML, la requête suivante recherche une valeur XML contenant le mot « custom » dans le titre d'un livre :  
  
```  
SELECT *   
FROM   T   
WHERE  CONTAINS(xCol,'custom')   
AND    xCol.exist('/book/title/text()[contains(.,"custom")]') =1  
```  
  
 La méthode **contains()** utilise l’index de texte intégral pour créer un sous-ensemble de valeurs XML contenant le mot « custom » à un endroit quelconque du document. La clause **exist()** vérifie que le mot « custom » apparaît dans le titre d’un livre.  
  
 Une recherche en texte intégral qui utilise **contains()** et la fonction XQuery **contains()** ont des sémantiques différentes. La dernière cherche à établir une concordance de sous-chaîne tandis que la première fait appel à la correspondance de mot clé avec extraction de radical. Par conséquent, si la recherche porte sur la chaîne où figure « run » dans le titre, les équivalences incluront « run », « runs » et « running » puisque la recherche en texte intégral **contains()** et la fonction Xquery **contains()** sont toutes deux satisfaites. En revanche, la requête ne trouve pas le mot « customizable » dans le titre, car la recherche en texte intégral **contains()** échoue et la fonction Xquery **contains()** est satisfaite. Généralement, pour une pure concordance de sous-chaîne, la clause **contains()** de texte intégral doit être supprimée.  
  
 De plus, la recherche en texte intégral se sert de l’extraction de radical tandis que la fonction XQuery **contains()** attend une correspondance littérale. Cette différence est expliquée dans l'exemple suivant.  
  
## <a name="example-full-text-search-on-xml-values-using-stemming"></a>Exemple : recherche en texte intégral sur des valeurs XML à l'aide de l'extraction de radical  
 La vérification XQuery **contains()** exécutée dans l’exemple précédent ne peut généralement pas être éliminée. Prenons par exemple la requête suivante :  
  
```  
SELECT *   
FROM   T   
WHERE  CONTAINS(xCol,'run')   
```  
  
 Le mot « ran » du document correspond aux critères de recherche du fait de l'extraction de radical. De plus, le contexte de recherche n'est pas vérifié à l'aide de XQuery.  
  
 Lorsque le code XML est décomposé, à l'aide de AXSD, dans des colonnes relationnelles indexées en texte intégral, les requêtes XPath qui portent sur la vue XML ne lancent pas une recherche en texte intégral sur les tables sous-jacentes.  
  
## <a name="see-also"></a> Voir aussi  
 [Index XML &#40;SQL Server&#41;](../../relational-databases/xml/xml-indexes-sql-server.md)  
  
  
