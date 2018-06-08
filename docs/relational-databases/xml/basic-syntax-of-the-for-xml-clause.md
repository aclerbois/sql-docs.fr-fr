---
title: Syntaxe de base de la clause FOR XML | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: xml
ms.reviewer: ''
ms.suite: sql
ms.technology: xml
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- BINARY BASE64 directive
- ROOT directive
- FOR XML clause, BINARY BASE64 directive
- FOR XML clause, syntax
- FOR XML clause, ROOT directive
ms.assetid: df19ecbf-d28e-4e9c-aaa3-700f8bbd3be4
caps.latest.revision: 38
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: c1c3f501283deebb6917997101c280ea8308327b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="basic-syntax-of-the-for-xml-clause"></a>Syntaxe de base de la clause FOR XML
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]
  Le mode de la clause FOR XML peut être RAW, AUTO, EXPLICIT ou PATH. Il détermine la forme du code XML résultant.  
  
> [!IMPORTANT]  
>  La directive XMLDATA de l'option FOR XML est déconseillée. Utilisez la génération XSD en mode RAW et AUTO. Il n'existe aucune solution de remplacement pour la directive XMLDATA en mode EXPLICIT. [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
 Voici la syntaxe de base telle qu’elle est décrite dans [Clause FOR (Transact-SQL)](../../t-sql/queries/select-for-clause-transact-sql.md):  
  
```  
[ FOR { BROWSE | <XML> } ]  
<XML> ::=  
XML   
    {   
      { RAW [ ('ElementName') ] | AUTO }   
        [   
           <CommonDirectives>   
           [ , { XMLDATA | XMLSCHEMA [ ('TargetNameSpaceURI') ]} ]   
           [ , ELEMENTS [ XSINIL | ABSENT ]   
        ]  
      | EXPLICIT   
        [   
           <CommonDirectives>   
           [ , XMLDATA ]   
        ]  
      | PATH [ ('ElementName') ]   
        [   
           <CommonDirectives>   
           [ , ELEMENTS [ XSINIL | ABSENT ] ]  
        ]  
     }   
  
 <CommonDirectives> ::=   
   [ , BINARY BASE64 ]  
   [ , TYPE ]  
   [ , ROOT [ ('RootName') ] ]  
```  
  
## <a name="arguments"></a>Arguments  
 RAW[('*ElementName*)]  
 Prend le résultat de la requête et transforme chaque ligne du jeu de résultats en un élément XML comportant un identificateur générique \<row /, comme balise d’élément. Vous pouvez, si vous le souhaitez, spécifier un nom pour l'élément de ligne en cas d'utilisation de cette directive. Le code XML qui en résulte utilise l’ *ElementName* spécifié pour identifier l’élément de ligne généré pour chaque ligne. Pour plus d’informations, consultez [Utiliser le mode RAW avec FOR XML](../../relational-databases/xml/use-raw-mode-with-for-xml.md).  
  
 AUTO  
 Renvoie les résultats de la requête dans une arborescence XML simple et imbriquée. Chaque table dans la clause FROM pour laquelle au moins une colonne existe dans la clause SELECT est représentée comme un élément XML. Les colonnes listées dans la clause SELECT sont mappées vers les attributs d'éléments appropriés. Pour plus d’informations, consultez [Utiliser le mode AUTO avec FOR XML](../../relational-databases/xml/use-auto-mode-with-for-xml.md).  
  
 EXPLICIT  
 Spécifie que la forme de l'arborescence XML résultante est définie de manière explicite. Dans ce mode, les requêtes doivent être écrites d'une manière particulière afin que des informations complémentaires sur l'imbrication souhaitée soient spécifiées de manière explicite. Pour plus d’informations, consultez [Utiliser le mode EXPLICIT avec FOR XML](../../relational-databases/xml/use-explicit-mode-with-for-xml.md).  
  
 PATH  
 Constitue un moyen simple de mélanger les éléments et les attributs, et d'introduire des imbrications supplémentaires pour représenter des propriétés complexes. Vous pouvez utiliser des requêtes FOR XML en mode EXPLICIT pour construire cette sorte de XML à partir d'un ensemble de lignes, mais le mode PATH est une alternative plus simple aux requêtes en mode EXPLICIT souvent beaucoup plus lourdes à manier. Le mode PATH, allié à la possibilité d’écrire des requêtes FOR XML imbriquées et de faire appel à la directive TYPE pour renvoyer les instances de type **xml** , vous permet d’écrire des requêtes de moindre complexité. Il offre une alternative à l'écriture de la plupart des requêtes en mode EXPLICIT. Par défaut, le mode PATH génère un wrapper d’élément \<row> pour chaque ligne du jeu de résultats. Vous pouvez, si vous le souhaitez, spécifier un nom d'élément. Si telle est votre intention, le nom spécifié est utilisé comme nom d'élément du wrapper. Si vous fournissez une chaîne vide (FOR XML PATH ('')), aucun élément wrapper n'est généré. Pour plus d’informations, consultez [Utiliser le mode PATH avec FOR XML](../../relational-databases/xml/use-path-mode-with-for-xml.md).  
  
 XMLDATA  
 Indique qu'un schéma XDR (XML-Data Reduced) doit être renvoyé. Le schéma est ajouté au début du document sous la forme d'un schéma en ligne. Pour obtenir un exemple fonctionnel, consultez [Utiliser le mode RAW avec FOR XML](../../relational-databases/xml/use-raw-mode-with-for-xml.md).  
  
 XMLSCHEMA  
 Renvoie un schéma XML W3C (XSD) en ligne. Vous pouvez, si vous le souhaitez, spécifier un URI d'espace de noms cible en cas d'utilisation de cette directive de façon à ce que l'espace de noms spécifié soit renvoyé dans le schéma. Pour plus d’informations, consultez [Générer un schéma XSD Inline](../../relational-databases/xml/generate-an-inline-xsd-schema.md). Pour obtenir un exemple fonctionnel, consultez [Utiliser le mode RAW avec FOR XML](../../relational-databases/xml/use-raw-mode-with-for-xml.md).  
  
 ELEMENTS  
 Si l'option ELEMENTS est spécifiée, les colonnes sont renvoyées sous forme de sous-éléments. Sinon, elles sont mappées avec des attributs XML. Cette option n'est prise en charge que dans les modes RAW, AUTO et PATH. Vous pouvez, si vous le souhaitez, spécifier XSINIL ou ABSENT en cas d'utilisation de cette directive. XSINIL spécifie qu’un élément qui a un attribut **xsi:nil** défini avec la valeur True soit créé pour les valeurs de colonne NULL. Par défaut, ou lorsque ABSENT est spécifié avec ELEMENTS, aucun élément n'est créé pour des valeurs NULL. Pour obtenir un exemple fonctionnel, consultez [Utiliser le mode RAW avec FOR XML](../../relational-databases/xml/use-raw-mode-with-for-xml.md) et [Utiliser le mode AUTO avec FOR XML](../../relational-databases/xml/use-auto-mode-with-for-xml.md).  
  
 BINARY BASE64  
 Si l'option BINARY Base64 est spécifiée, toutes les données binaires renvoyées par la requête sont représentées dans un format encodé en base 64. Cette option doit être spécifiée pour l'extraction de données binaires en mode RAW et EXPLICIT. En mode AUTO, des données binaires sont renvoyées comme une référence par défaut. Pour obtenir un exemple fonctionnel, consultez [Utiliser le mode RAW avec FOR XML](../../relational-databases/xml/use-raw-mode-with-for-xml.md).  
  
 TYPE  
 Spécifie que la requête retourne des résultats de type **xml** . Pour plus d’informations, consultez [Directive TYPE dans les requêtes FOR XML](../../relational-databases/xml/type-directive-in-for-xml-queries.md).  
  
 ROOT [('*RootName*')]  
 Spécifie qu'un et un seul élément de premier niveau doit être ajouté au code XML résultant. Vous pouvez, si vous le souhaitez, spécifier le nom d'élément racine à générer. La valeur par défaut est « root ».  
  
## <a name="see-also"></a> Voir aussi  
 [Utiliser le mode RAW avec FOR XML](../../relational-databases/xml/use-raw-mode-with-for-xml.md)   
 [UTiliser le mode AUTO avec FOR XML](../../relational-databases/xml/use-auto-mode-with-for-xml.md)   
 [Utiliser le mode EXPLICIT avec FOR XML](../../relational-databases/xml/use-explicit-mode-with-for-xml.md)   
 [Utiliser le mode PATH avec FOR XML](../../relational-databases/xml/use-path-mode-with-for-xml.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [FOR XML &#40;SQL Server&#41;](../../relational-databases/xml/for-xml-sql-server.md)  
  
  
