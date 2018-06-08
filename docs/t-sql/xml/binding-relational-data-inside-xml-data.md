---
title: Liaison de données relationnelles dans des données XML | Microsoft Docs
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: t-sql|xml
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- relational data binding [SQL Server]
- XML [SQL Server], binding relational data
- xml data type [SQL Server], relational data binding
- binding relational data [XML in SQL Server]
- variables [XML in SQL Server], relational data binding
- columns [XML in SQL Server], relational data binding
ms.assetid: 03d013a9-b53f-46c3-9628-da77f099c74a
caps.latest.revision: 36
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: db924e662687ab79d207fe3e1e33ccc75aecd059
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="binding-relational-data-inside-xml-data"></a>Liaison de données relationnelles dans des données XML
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  Vous pouvez spécifier des [méthodes de type de données xml](../../t-sql/xml/xml-data-type-methods.md) sur une variable ou une colonne de type de données **xml**. Par exemple, la [méthode query&#40; &#41; &#40;type de données xml&#41;](../../t-sql/xml/query-method-xml-data-type.md) exécute la requête Xml spécifiée sur une instance XML. Lorsque vous construisez des données XML de cette manière, vous pouvez inclure une valeur à partir d'une colonne de type non-XML ou une variable Transact-SQL. Ce processus est appelé liaison de données relationnelles dans des données XML.  
  
 Pour lier les données relationnelles non-XML dans des données XML, le moteur de base de données SQL Server fournit les pseudo-fonctions suivantes :  
  
-   [Fonction sql:column&#40;&#41; &#40;requête Xml&#41;](../../xquery/xquery-extension-functions-sql-column.md) Vous permet d’utiliser les valeurs d’une colonne relationnelle dans votre requête Xml ou expression DML XML.  
  
-   [Fonction sql:variable&#40;&#41; &#40;requête Xml&#41;](../../xquery/xquery-extension-functions-sql-variable.md). Vous permet d'utiliser la valeur d'une variable SQL dans votre expression XQuery ou DML XML.  
  
 Vous pouvez utiliser ces fonctions avec les méthodes de type de données **xml** chaque fois que vous souhaitez exposer une valeur relationnelle dans du code XML.  
  
 Vous ne pouvez pas utiliser ces fonctions pour référencer des données dans des colonnes ou variables de type **xml**, CLR défini par l’utilisateur, datetime, smalldatetime, **text**, **ntext**, **sql_variant** et **image**.  
  
 En outre, cette liaison est destinée à un emploi en lecture seule. Cela signifie que vous ne pouvez pas écrire de données dans les colonnes qui utilisent ces fonctions. Par exemple, l’instruction sql:variable("@x")="*expression"* n’est pas autorisée.  
  
## <a name="example-cross-domain-query-using-sqlvariable"></a>Exemple : requête inter-domaines utilisant sql:variable()  
 Cet exemple montre comment **sql:variable()** peut permettre à une application de paramétrer une requête. Le numéro ISBN est passé en utilisant une variable SQL @isbn. En remplaçant la constante par **sql:variable()**, vous pouvez utiliser la requête pour rechercher n’importe quel numéro ISBN et pas seulement le numéro 0-7356-1588-2.  
  
```  
DECLARE @isbn varchar(20)  
SET     @isbn = '0-7356-1588-2'  
SELECT  xCol  
FROM    T  
WHERE   xCol.exist ('/book/@ISBN[. = sql:variable("@isbn")]') = 1  
```  
  
 **sql:column()** peut être utilisée de manière similaire et apporter d’autres avantages. Pour gagner en efficacité, vous pouvez placer des index sur la colonne, en suivant les suggestions de l'optimiseur de requête basé sur les coûts. De plus, la colonne calculée peut stocker une propriété promue.  
  
## <a name="see-also"></a> Voir aussi  
 [Méthodes de type de données xml](../../t-sql/xml/xml-data-type-methods.md)  
  
  
