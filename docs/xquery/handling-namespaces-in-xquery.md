---
title: La gestion des espaces de noms dans XQuery | Documents Microsoft
ms.custom: ''
ms.date: 03/07/2017
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
- declaring namespaces
- namespaces [XQuery]
- XQuery, namespaces
ms.assetid: 542b63da-4d3d-4ad5-acea-f577730688f1
caps.latest.revision: 24
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: 4602c1234c00b15191ca616ed56352f0eb784d9a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="handling-namespaces-in-xquery"></a>Gestion des espaces de noms dans XQuery
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

  Cette rubrique fournit des exemples qui illustrent comment gérer les espaces de noms dans les requêtes.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-declaring-a-namespace"></a>A. Déclaration d'un espace de noms  
 La requête suivante récupère les étapes de fabrication d'un modèle de produit spécifique.  
  
```  
SELECT Instructions.query('  
     declare namespace AWMI="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
        /AWMI:root/AWMI:Location[1]/AWMI:step  
    ') as x  
FROM Production.ProductModel  
WHERE ProductModelID=7  
```  
  
 Voici le résultat partiel :  
  
```  
<AWMI:step xmlns:AWMI="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions">Insert <AWMI:material>aluminum sheet MS-2341</AWMI:material> into the <AWMI:tool>T-85A framing tool</AWMI:tool>. </AWMI:step>  
…  
```  
  
 Notez que la **espace de noms** (mot clé) est utilisé pour définir un nouveau préfixe d’espace de noms, « AWMI : ». Ce préfixe doit ensuite être utilisé dans la requête pour tous les éléments couverts par l'étendue de cet espace de noms.  
  
### <a name="b-declaring-a-default-namespace"></a>B. Déclaration d'un espace de noms par défaut  
 Dans la requête précédente, un nouveau préfixe d'espace de noms a été défini. Il a fallu ensuite utiliser ce préfixe dans la requête pour sélectionner les structures XML appropriées. Une alternative consiste à déclarer un espace de noms par défaut, comme le montre la requête modifiée suivante :  
  
```  
SELECT Instructions.query('  
     declare default element namespace "http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
        /root/Location[1]/step  
    ') as x  
FROM Production.ProductModel  
where ProductModelID=7  
```  
  
 Voici le résultat :  
  
```  
<step xmlns="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions">Insert <material>aluminum sheet MS-2341</material> into the <tool>T-85A framing tool</tool>. </step>  
…  
```  
  
 Notez, dans cet exemple, que l'espace de noms défini, `"http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions"`, est créé pour remplacer l'espace de noms par défaut ou vide. Grâce à cela, vous pouvez vous passer d'un préfixe d'espace de noms dans l'expression de chemin utilisée lors de l'interrogation. Le préfixe d'espace de noms devient également inutile dans les noms d'élément qui apparaissent dans les résultats. De plus, l'espace de noms par défaut s'applique à tous les éléments, mais pas à leurs attributs.  
  
### <a name="c-using-namespaces-in-xml-construction"></a>C. Utilisation des espaces de noms dans la construction XML  
 Lorsque vous définissez de nouveaux espaces de noms, ils ont un rôle à jouer non seulement dans la requête, mais aussi dans la construction. Lors de la construction XML par exemple, vous pouvez définir un nouvel espace de noms à l'aide de la déclaration « `declare namespace ...` », puis l'utiliser avec les éléments et les attributs que vous souhaitez faire apparaître dans les résultats de la requête.  
  
```  
SELECT CatalogDescription.query('  
     declare default element namespace "http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
     declare namespace myNS="uri:SomeNamespace";<myNS:Result>  
          { /ProductDescription/Summary }  
       </myNS:Result>  
  
    ') as Result  
FROM Production.ProductModel  
where ProductModelID=19  
```  
  
 Voici le résultat obtenu :  
  
```  
  
      <myNS:Result xmlns:myNS="uri:SomeNamespace">  
  <Summary xmlns="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription">  
   <p1:p xmlns:p1="http://www.w3.org/1999/xhtml">  
     Our top-of-the-line competition mountain bike. Performance-enhancing   
     options include the innovative HL Frame, super-smooth front   
     suspension, and traction for all terrain.</p1:p>  
  </Summary>  
</myNS:Result>  
```  
  
 Vous pouvez également définir l'espace de noms explicitement à chaque stade où il intervient dans la construction XML, comme le montre la requête suivante :  
  
```  
SELECT CatalogDescription.query('  
     declare default element namespace "http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
       <myNS:Result xmlns:myNS="uri:SomeNamespace">  
          { /ProductDescription/Summary }  
       </myNS:Result>  
    ') as Result  
FROM Production.ProductModel  
where ProductModelID=19  
```  
  
### <a name="d-construction-using-default-namespaces"></a>D. Construction à l'aide des espaces de noms par défaut  
 Vous pouvez aussi définir un espace de noms par défaut en vue de son utilisation dans le code XML construit. Par exemple, la requête suivante montre comment vous pouvez spécifier un espace de noms par défaut, « SomeNamespace »\\, à utiliser en tant que la valeur par défaut pour les éléments nommés localement qui sont construits, comme le `<Result>` élément.  
  
```  
SELECT CatalogDescription.query('  
      declare namespace PD="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
      declare default element namespace "uri:SomeNamespace";<Result>  
          { /PD:ProductDescription/PD:Summary }  
       </Result>  
  
    ') as Result  
FROM Production.ProductModel  
where ProductModelID=19  
```  
  
 Voici le résultat obtenu :  
  
```  
  
      <Result xmlns="uri:SomeNamespace">  
  <PD:Summary xmlns:PD="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription">  
   <p1:p xmlns:p1="http://www.w3.org/1999/xhtml">  
         Our top-of-the-line competition mountain bike. Performance-  
         enhancing options include the innovative HL Frame, super-smooth   
         front suspension, and traction for all terrain.</p1:p>  
  </PD:Summary>  
</Result>  
```  
  
 Notez qu'en remplaçant l'espace de noms d'élément par défaut ou l'espace de noms vide, tous les éléments nommés localement dans le code XML construit sont par la suite liés à l'espace de noms par défaut de substitution. Par conséquent, si vous voulez profiter de la souplesse d'un espace de noms vide lors de la construction XML, évitez de remplacer l'espace de noms d'élément par défaut.  
  
## <a name="see-also"></a>Voir aussi  
 [Ajouter des espaces de noms aux requêtes avec WITH XMLNAMESPACES](../relational-databases/xml/add-namespaces-to-queries-with-with-xmlnamespaces.md)   
 [Données XML &#40;SQL Server&#41;](../relational-databases/xml/xml-data-sql-server.md)   
 [Références relatives au langage Xquery &#40;SQL Server&#41;](../xquery/xquery-language-reference-sql-server.md)  
  
  
