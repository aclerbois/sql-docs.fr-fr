---
title: Modifier les index XML | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: xml
ms.reviewer: ''
ms.suite: sql
ms.technology: xml
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- XML indexes [SQL Server], modifying
- modifying indexes
ms.assetid: 24d50fe1-c6ec-49e6-91a3-9791851ba53d
caps.latest.revision: 12
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 87e16b7c52a01b3755a5e7fb7d232f937470953c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="modify-xml-indexes"></a>Modifier les index XML
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]
  L’instruction DDL [!INCLUDE[tsql](../../includes/tsql-md.md)] [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md) permet de modifier des index XML et non XML existants. Les options ALTER INDEX ne sont cependant pas toutes disponibles aux index XML. Les options suivantes ne sont pas valides si vous modifiez des index XML :  
  
-   L'option de reconstruction et de définition de valeur IGNORE_DUP_KEY n'est pas valide dans le cas d'utilisation d'index XML. L'option de reconstruction ONLINE doit toujours être définie sur OFF dans le cas d'index XML secondaires. L'option DROP_EXISTING n'est pas autorisée dans l'instruction ALTER INDEX.  
  
-   Les modifications apportées à la contrainte de clé primaire de la table utilisateur ne se transmettent pas automatiquement aux index XML. L'utilisateur doit dans ce cas supprimer les index XML en premier, puis les recréer.  
  
-   Si l'option ALTER INDEX ALL est précisée, elle s'applique aussi bien aux index non XML qu'aux index XML. Il se peut que les options d'indexation soient indiquées comme n'étant pas valides pour ces deux types d'index. Dans ce cas, l'instruction entière est ignorée.  
  
## <a name="example-modifying-an-xml-index"></a>Exemple : modification d'un index XML  
 Dans l'exemple suivant, un index XML est créé puis modifié en affectant la valeur `ALLOW_ROW_LOCKS` à l'option `OFF`. Lorsque l'option `ALLOW_ROW_LOCKS` est ainsi définie sur `OFF`, les lignes ne sont pas verrouillées et l'accès aux index spécifiés est obtenu au moyen de verrous de niveau page et table.  
  
```  
CREATE TABLE T (Col1 INT PRIMARY KEY, XmlCol XML)  
GO  
-- Create primary XML index.   
CREATE PRIMARY XML INDEX PIdx_T_XmlCol   
ON T(XmlCol)  
GO  
-- Note the type 3 is index on XML type.  
SELECT *  
FROM sys.xml_indexes  
WHERE object_id = object_id('T')  
AND name='PIdx_T_XmlCol'  
  
-- Modify and set an index option.  
ALTER INDEX PIdx_T_XmlCol on T   
SET (ALLOW_ROW_LOCKS = OFF)  
```  
  
## <a name="example-disabling-and-enabling-an-xml-index"></a>Exemple: désactivation et activation d'un index XML  
 Par défaut, un index XML est activé. S'il est désactivé par la suite, les requêtes sur la colonne XML n'utilisent alors pas l'index XML. Pour réactiver un index XML, passez par l'instruction `ALTER INDEX` avec l'option `REBUILD` .  
  
```  
CREATE TABLE T (Col1 INT PRIMARY KEY, XmlCol XML)  
GO  
CREATE PRIMARY XML INDEX PIdx_T_XmlCol ON T(XmlCol)  
GO  
ALTER INDEX PIdx_T_XmlCol on T DISABLE  
Go  
-- Verify index is disabled.  
SELECT *  
FROM sys.xml_indexes  
WHERE object_id = object_id('T')  
AND name='PIdx_T_XmlCol'  
-- Rebuild the index.  
ALTER INDEX PIdx_T_XmlCol on T REBUILD  
Go  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Index XML &#40;SQL Server&#41;](../../relational-databases/xml/xml-indexes-sql-server.md)  
  
  
