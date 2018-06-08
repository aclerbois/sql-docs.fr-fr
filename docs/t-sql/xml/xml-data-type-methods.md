---
title: Méthodes de type de données xml | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: sql-database
ms.component: t-sql|xml
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- xml data type [SQL Server], methods
- methods [XML in SQL Server]
ms.assetid: d112b9c9-be9f-435c-a9e6-d21b65778fb7
caps.latest.revision: 32
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: b788282072088b4c9ee98b27b6f4ce3fb2b923c6
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="xml-data-type-methods"></a>Méthodes des types de données xml
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

  Vous pouvez utiliser les méthodes de type de données **xml** pour interroger une instance XML stockée dans une variable ou colonne de type **xml**. Les rubriques de cette section expliquent comment utiliser les méthodes de type de données **xml**.  
  
## <a name="in-this-section"></a>Dans cette section  
  
|Rubrique|Description|  
|-----------|-----------------|  
|[Méthode query&#40;&#41; &#40;type de données xml &#41;](../../t-sql/xml/query-method-xml-data-type.md)|Décrit comment utiliser la méthode query() pour interroger une instance XML.|  
|[Méthode value&#40;&#41; &#40;type de données xml&#41;](../../t-sql/xml/value-method-xml-data-type.md)|Décrit comment utiliser la méthode value() pour récupérer une valeur de type SQL d'une instance XML.|  
|[Méthode exist&#40;&#41; &#40;type de données xml&#41;](../../t-sql/xml/exist-method-xml-data-type.md)|Décrit comment utiliser la méthode exist() pour déterminer si une requête retourne un résultat non vide.|  
|[Méthode modify&#40;&#41; &#40;type de données xml&#41;](../../t-sql/xml/modify-method-xml-data-type.md)|Décrit comment utiliser la méthode modify() pour spécifier des instructions en [langage de manipulation de données XML &#40;DML XML&#41;](../../t-sql/xml/xml-data-modification-language-xml-dml.md) permettant d’effectuer des mises à jour.|  
|[Méthode nodes&#40;&#41; &#40;type de données xml&#41;](../../t-sql/xml/nodes-method-xml-data-type.md)|Décrit comment utiliser la méthode nodes() pour fragmenter du code XML en plusieurs lignes, ce qui permet de propager des parties de documents XML dans des ensembles de lignes.|  
|[Liaison de données relationnelles dans des données XML](../../t-sql/xml/binding-relational-data-inside-xml-data.md)|Décrit comment lier des données non-XML à l'intérieur de code XML.|  
|[Instructions pour l’utilisation des méthodes de type de données XML](../../t-sql/xml/guidelines-for-using-xml-data-type-methods.md)|Fournit des instructions sur l’utilisation des méthodes de type de données **xml**.|  
  
 Vous appelez ces méthodes au moyen de la syntaxe d'appel de méthode de type défini par l'utilisateur. Exemple :  
  
```  
SELECT XmlCol.query(' ... ')  
FROM   Table  
```  
  
> [!NOTE]  
>  Les méthodes de type de données **xml** **query()**, **value()** et **exist()** retournent NULL si elles sont exécutées sur une instance XML NULL. De plus, **modify()** ne retourne rien, mais **nodes()** retourne des ensembles de lignes avec un ensemble de lignes vide avec une entrée NULL.  
  
## <a name="see-also"></a> Voir aussi  
 [Comparer du XML typé et du XML non typé](../../relational-databases/xml/compare-typed-xml-to-untyped-xml.md)   
 [Créer des instances de données XML](../../relational-databases/xml/create-instances-of-xml-data.md)  
  
  
