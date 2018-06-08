---
title: WITH XMLNAMESPACES (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: sql-database
ms.component: t-sql|xml
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- WITH_XMLNAMESPACES_TSQL
- WITH XMLNAMESPACES
dev_langs:
- TSQL
helpviewer_keywords:
- adding XML namespaces
- XML namespace declarations [SQL Server]
- clauses [SQL Server], WITH XMLNAMESPACES
- WITH XMLNAMESPACES clause
- declaring XML namespaces
ms.assetid: 3b32662b-566f-454d-b7ca-e247002a9a0b
caps.latest.revision: 17
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 7e7e6763f0cbd2228e9781876fe198e529e0dedc
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="with-xmlnamespaces"></a>WITH XMLNAMESPACES
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Déclare un ou plusieurs espaces de noms XML.  
  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
WITH XMLNAMESPACES ( <XML namespace declaration item>  
[ { , <XML namespace declaration item> }...] )   
  
<XML namespace declaration item> ::=  
<xml_namespace_uri> AS <xml_namespace_prefix>  
| <XML default namespace declaration item>  
<xml_namespace_uri> ::= <character string literal>  
```  
  
```  
  
<xml_namespace_prefix> ::= <identifier>  
```  
  
```  
  
<XML default namespace declaration item> ::=  
DEFAULT <xml_namespace_uri>  
  
```  
  
## <a name="arguments"></a>Arguments  
 *xml_namespace_uri*  
 URI (Uniform Resource Identifier) identifiant l'espace de noms XML déclaré. *xml_namespace_uri* est une chaîne SQL.  
  
 *xml_namespace_prefix*  
 Spécifie un préfixe à mapper sur l’URI de l’espace de noms associé indiqué dans *xml_namespace_uri*. *xml_namespace_prefix* doit être un identificateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="remarks"></a>Notes   
 Lorsque vous utilisez la clause WITH XMLNAMESPACES dans une instruction qui comprend également une expression de table commune, la clause doit précéder l'expression dans l'instruction.  
  
 Les règles générales de syntaxe suivantes s'appliquent lorsque vous utilisez la clause WITH XMLNAMESPACES :  
  
-   Chaque déclaration d'espace de noms XML doit contenir au moins un élément de déclaration d'espace de noms XML par défaut.  
  
-   Chaque préfixe d'espace de noms XML utilisé doit être un nom « non colonisé » (NCName) dans lequel le caractère deux-points (:) ne fait pas partie du nom.  
  
-   Vous ne pouvez pas définir deux fois un préfixe d'espace de noms.  
  
-   Les préfixes et les URI d'espace de noms XML respectent la casse.  
  
-   Le préfixe d'espace de noms XML `xmlns` ne peut pas être déclaré.  
  
-   Le préfixe d'espace de noms XML `xml` ne peut pas être remplacé par un espace de nom autre que l'URI `'http://www.w3.org/XML/1998/namespace'`, et cet URI ne peut pas recevoir un préfixe différent.  
  
-   Le préfixe d'espace de noms XML `xsi` ne peut pas être redéclaré lorsque la directive ELEMENTS XSINIL est utilisée sur la requête.  
  
-   Les valeurs des chaînes URI sont encodées conformément à la page de codes de classement de la base de données actuelle et elles sont converties au format Unicode.  
  
-   L’URI de l’espace de noms XML est soumis aux règles de réduction des blancs XSD utilisées pour **xs:anyURI**. Notez également que les valeurs des URI d'espace de noms ne subissent ni décomposition en entités, ni regroupement des entités.  
  
-   Une vérification de l'URI d'espace de noms XML permet de repérer les caractères XML 1.0 qui ne sont pas valides et de générer une erreur le cas échéant (par exemple, si le caractère U+0007 est détecté).  
  
-   L'URI d'espace de noms XML (après réduction de tous les blancs) ne peut pas être une chaîne de longueur nulle car cela déclenche une erreur « invalid empty namespace URI ».  
  
-   Le mot clé XMLNAMESPACES est réservé dans le contexte de la clause WITH.  
  
## <a name="examples"></a>Exemples  
 Pour obtenir des exemples, consultez [Ajouter des espaces de noms aux requêtes avec WITH XMLNAMESPACES](../../relational-databases/xml/add-namespaces-to-queries-with-with-xmlnamespaces.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Références relatives au langage Xquery &#40;SQL Server&#41;](../../xquery/xquery-language-reference-sql-server.md)  
  
  
