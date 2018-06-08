---
title: Séquence et QNames (XQuery) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
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
- sequence [XQuery]
- XQuery, sequence
- QName [XQuery]
- predefined namespaces [XML in SQL Server]
ms.assetid: 3593ac26-dd78-4bf0-bb87-64fbcac5f026
caps.latest.revision: 22
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: 30d463050f129bbc232c0261f1d6af481744ef93
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sequence-and-qnames-xquery"></a>Séquence et QNames (XQuery)
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

  Cette rubrique décrit les concepts fondamentaux suivants de XQuery :  
  
-   Séquence  
  
-   QNames et espaces de noms prédéfinis  
  
## <a name="sequence"></a>Séquence  
 Dans XQuery, le résultat d'une expression est une séquence composée d'une liste de nœuds XML et d'instances de types atomiques XSD. Une entrée d'une séquence porte le nom d'élément. Un élément d'une séquence peut être :  
  
-   un nœud, tel qu'un élément, attribut, texte, instruction de traitement, commentaire ou document ;  
  
-   une valeur atomique, telle qu'une instance d'un type simple XSD.  
  
 Par exemple, la requête suivante construit une séquence de deux éléments de nœud d'élément :  
  
```  
SELECT Instructions.query('  
     <step1> Step 1 description goes here</step1>,  
     <step2> Step 2 description goes here </step2>  
') AS Result  
FROM Production.ProductModel  
WHERE ProductModelID=7;  
  
```  
  
 Voici le résultat obtenu :  
  
```  
<step1> Step 1 description goes here </step1>  
<step2> Step 2 description goes here </step2>   
```  
  
 Dans la requête précédente, la virgule (`,`) à la fin de la construction `<step1>` est le constructeur de séquence ; elle est obligatoire. Les espaces dans les résultats sont ajoutés uniquement à titre d'illustration et sont inclus dans tous les résultats d'exemple de cette documentation.  
  
 Voici quelques informations supplémentaires utiles concernant les séquences :  
  
-   Si une requête génère une séquence qui contient une autre séquence, la séquence contenue est aplatie dans la séquence conteneur. Par exemple, la séquence ((1,2, (3,4,5)),6) est aplatie dans le modèle de données sous la forme (1, 2, 3, 4, 5, 6).  
  
    ```  
    DECLARE @x xml;  
    SET @x = '';  
    SELECT @x.query('(1,2, (3,4,5)),6');  
    ```  
  
-   Une séquence vide est une séquence qui ne contient aucun élément. Elle est représentée sous la forme « () ».  
  
-   Une séquence contenant un seul élément peut être traitée comme une valeur atomique, et inversement. À savoir, (1) = 1.  
  
 Dans cette implémentation, la séquence doit être homogène. Autrement dit, vous devez avoir soit une séquence de valeurs atomiques, soit une séquence de nœuds. Par exemple, les séquences suivantes sont valides :  
  
```  
DECLARE @x xml;  
SET @x = '';  
-- Expression returns a sequence of 1 text node (singleton).  
SELECT @x.query('1');  
-- Expression returns a sequence of 2 text nodes  
SELECT @x.query('"abc", "xyz"');  
-- Expression returns a sequence of one atomic value. data() returns  
-- typed value of the node.  
SELECT @x.query('data(1)');  
-- Expression returns a sequence of one element node.   
-- In the expression XML construction is used to construct an element.  
SELECT @x.query('<x> {1+2} </x>');  
```  
  
 La requête suivante retourne une erreur car les séquences hétérogènes ne sont pas prises en charge.  
  
```  
SELECT @x.query('<x>11</x>, 22');  
```  
  
## <a name="qname"></a>QName  
 Chaque identificateur d'une XQuery est un QName. Un QName est constitué d'un préfixe d'espace de noms et d'un nom local. Dans cette implémentation, les noms de variables dans XQuery sont des QNames et ne peuvent pas avoir de préfixes.  
  
 Prenons l’exemple suivant dans lequel une requête est spécifiée par rapport à un non typé **xml** variable :  
  
```  
DECLARE @x xml;  
SET @x = '<Root><a>111</a></Root>';  
SELECT @x.query('/Root/a');  
```  
  
 Dans l'expression (`/Root/a`), `Root` et `a` sont des QNames.  
  
 Dans l’exemple suivant, une requête est spécifiée contre un typé **xml** colonne. La requête effectue une itération sur tous les \<étape > éléments au premier atelier.  
  
```  
SELECT Instructions.query('  
   declare namespace AWMI="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
for $Step in /AWMI:root/AWMI:Location[1]/AWMI:step  
      return  
           string($Step)   
') AS Result  
FROM Production.ProductModel  
WHERE ProductModelID=7;  
```  
  
 Dans l'expression de requête, notez ce qui suit :  
  
-   `AWMI root`, `AWMI:Location`, `AWMI:step` et `$Step` sont tous des QNames. `AWMI` est un préfixe et `root`, `Location` et `Step` sont tous des noms locaux.  
  
-   La variable `$step` est un QName ; elle n'a pas de préfixe.  
  
 Les espaces de noms suivants sont prédéfinis pour une utilisation avec prise en charge de XQuery dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].  
  
|Prefix|URI|  
|------------|---------|  
|xs|http://www.w3.org/2001/XMLSchema|  
|xsi|http://www.w3.org/2001/XMLSchema-instance|  
|xdt|http://www.w3.org/2004/07/xpath-datatypes|  
|fn|http://www.w3.org/2004/07/xpath-functions|  
|(aucun préfixe)|`urn:schemas-microsoft-com:xml-sql`|  
|sqltypes|http://schemas.microsoft.com/sqlserver/2004/sqltypes|  
|xml|`http://www.w3.org/XML/1998/namespace`|  
|(aucun préfixe)|`http://schemas.microsoft.com/sqlserver/2004/SOAP`|  
  
 Chaque base de données que vous créez a la **sys** collection de schémas XML. Elle réserve ces schémas de sorte qu'ils soient accessibles à partir de toute collection de schémas XML créée par l'utilisateur.  
  
> [!NOTE]  
>  Cette implémentation ne prend pas en charge la `local` préfixe comme décrit dans la spécification XQuery dans http://www.w3.org/2004/07/xquery-local-functions.  
  
## <a name="see-also"></a>Voir aussi  
 [Concepts de base de XQuery](../xquery/xquery-basics.md)  
  
  
