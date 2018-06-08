---
title: Langage de manipulation de données XML (DML XML) | Microsoft Docs
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
- modifying data [SQL Server], XML DML
- XML [SQL Server], DML
- XML DML [SQL Server]
- data modification language [XML DML]
- data modifications [XML DML]
- DML [XML in SQL Server]
- XQuery, XML DML
- xml data type [SQL Server], XML DML
ms.assetid: 20ce50d2-c07b-4e41-93a7-1380d2cd49cb
caps.latest.revision: 29
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 61d60da9c7cf14fd00bc00bf4d55cbb1ebedf141
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="xml-data-modification-language-xml-dml"></a>Langage de modification de données XML (XML DML)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Le langage de modification de données XML (XML DML) est une extension du langage XQuery. Tel qu'il est défini par le consortium W3C, le langage XQuery ne possède pas la partie de manipulation des données (DML). Le langage de manipulation de données (DML, Data Manipulation Language) XML présenté dans cette rubrique, ainsi que le langage de requête Xml, fournissent un langage de modification de données et de requête pleinement fonctionnel que vous pouvez utiliser sur les données de type **xml**.  
  
 Le XML DML ajoute dans XQuery les mots clés respectant la casse suivants :  
  
-   **insert**  
  
-   **delete**  
  
-   **replace value of**  
  
 Comme décrit dans [Type et colonnes de données XML &#40;SQL Server&#41;](../../relational-databases/xml/xml-data-type-and-columns-sql-server.md), vous pouvez créer des variables et des colonnes de type **xml** et leur attribuer des documents ou fragments XML. Pour modifier ou mettre à jour ces instances XML, procédez comme suit :  
  
-   Utilisez la [méthode modify()](../../t-sql/xml/modify-method-xml-data-type.md) du type de données **xml**.  
  
-   Spécifiez les instructions DML XML appropriées dans la méthode **modify()**.  
  
 Notez que certains attributs ne peuvent pas être insérés ou supprimés, et que leur valeur ne peut pas être modifiée. Exemple :  
  
-   Pour le **xml** typé et non typé, les attributs sont **xmlns**, **xmlns:\*** et **xml:base**.  
  
-   Pour le **xml** typé uniquement, les attributs sont **xsi:nil** et **xsi:type**.  
  
 Autres restrictions :  
  
-   Pour le **xml** typé et non typé, l’insertion de l’attribut **xml:base** échoue.  
  
-   Pour le **xml** typé, la suppression et la modification de l’attribut **xsi:nil** échouent. Pour le **xml** non typé, vous pouvez supprimer l’attribut ou modifier sa valeur.  
  
-   Pour le **xml** typé, la modification de la valeur de l’attribut **xs:type** échoue. Pour le **xml** non typé, vous pouvez modifier la valeur de l’attribut.  
  
 Lorsque vous modifiez une instance XML typée, le format final doit être une instance valide de ce type. Dans le cas contraire, une erreur de validation est retournée.  
  
## <a name="see-also"></a> Voir aussi  
 [insert &#40;DML XML&#41;](../../t-sql/xml/insert-xml-dml.md)   
 [delete &#40;DML XML&#41;](../../t-sql/xml/delete-xml-dml.md)   
 [replace value of &#40;DML XML&#41;](../../t-sql/xml/replace-value-of-xml-dml.md)   
 [Comparer du XML typé et du XML non typé](../../relational-databases/xml/compare-typed-xml-to-untyped-xml.md)   
 [Créer des instances de données XML](../../relational-databases/xml/create-instances-of-xml-data.md)   
 [Méthodes de type de données xml](../../t-sql/xml/xml-data-type-methods.md)  
  
  
