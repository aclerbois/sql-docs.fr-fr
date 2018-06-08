---
title: Types de données XPath (SQLXML 4.0) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: sqlxml
ms.reviewer: ''
ms.suite: sql
ms.technology: xml
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- mapping XDR types to XPath types [SQLXML]
- data types [XPath]
- arithmetic operators
- mapping data types [SQLXML]
- relational operators [SQLXML]
- node-set [SQLXML]
- data types [SQLXML], XPath
- XPath operators [SQLXML]
- XDR data type [SQLXML]
- equality operators [SQLXML]
- XPath conversions [SQLXML]
- converting data types [SQLXML]
- Boolean operators
- XPath queries [SQLXML], data types
- XPath data types [SQLXML]
- operators [SQLXML]
ms.assetid: a90374bf-406f-4384-ba81-59478017db68
caps.latest.revision: 27
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 7762ba02f4368b64fc4073936cb04c293029e9d2
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="xpath-data-types-sqlxml-40"></a>Types de données XPath (SQLXML 4.0)
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], XPath et XML Schema (XSD) sont dotés de types de données très différents. Par exemple, XPath n'affiche aucun type de données integer ou date tandis que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et XSD en possèdent un grand nombre. XSD utilise une précision à la nanoseconde pour les valeurs temporelles ; [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] affiche au maximum une précision de 1/300ème de seconde. Par conséquent, le mappage d'un type de données à un autre n'est pas toujours possible. Pour plus d’informations sur le mappage [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] des types de données aux types de données XSD, consultez [forçages de Type de données et de l’Annotation SQL : DataType &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/data-type-coercions-and-the-sql-datatype-annotation-sqlxml-4-0.md).  
  
 XPath possède trois types de données : **chaîne**, **nombre**, et **booléenne**. Le **nombre** type de données est toujours une IEEE 754 virgule flottante double précision. Le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **float (53)** type de données est le plus proche XPath **nombre**. Toutefois, **float (53)** n’est pas exactement IEEE 754. Par exemple, ni la valeur NaN (Not-a-Number,), ni une valeur infinie n'est employée. Tente de convertir une chaîne non numérique en **nombre** et la tentative de division par zéro génère une erreur.  
  
## <a name="xpath-conversions"></a>Conversions XPath  
 Lorsque vous utilisez une requête XPath, telle que `OrderDetail[@UnitPrice > "10.0"]`, les conversions de type de données implicites et explicites peuvent modifier la signification de la requête de manière subtile. Par conséquent, il est primordial de comprendre la manière dont les types de données Xpath sont implémentés. La spécification du langage XPath, XML Path Language (XPath) version 1.0 W3C Proposed Recommendation du 8 octobre 1999, sont accessibles sur le site Web de W3C à http://www.w3.org/TR/1999/PR-xpath-19991008.html.  
  
 Les opérateurs XPath sont divisés en quatre catégories :  
  
-   Opérateurs booléens (et, ou)  
  
-   Opérateurs relationnels (\<, >, \<=, > =)  
  
-   Opérateurs d'égalité (=, !=)  
  
-   Opérateurs arithmétiques (+, -, *, div, mod)  
  
 Chaque catégorie d'opérateur convertit ses opérandes de manière distincte. Les opérateurs XPath convertissent implicitement leurs opérandes si cela est nécessaire. Opérateurs arithmétiques convertissent leurs opérandes en **nombre**et le résultat d’une valeur numérique. Opérateurs booléens convertissent leurs opérandes en **booléenne**et le résultat d’une valeur booléenne. Les opérateurs relationnels et d'égalité génèrent une valeur booléenne. Toutefois, ils suivent des règles de conversion différentes en fonction des types de données d'origine de leurs opérandes comme le montre le tableau ci-après.  
  
|Opérande|Opérateur relationnel|Opérateur d'égalité|  
|-------------|-------------------------|-----------------------|  
|Les deux opérandes sont des éléments node-set.|TRUE si et seulement s’il existe un nœud dans un jeu et un nœud dans le deuxième jeu justifiant que la comparaison de leurs **chaîne** valeurs a la valeur TRUE.|Idem.|  
|Un est un élément node-set, l’autre un **chaîne**.|TRUE si et seulement s’il existe un nœud dans l’ensemble de nœuds tels que lorsque converti en **nombre**, sa comparaison avec le **chaîne** converti en **nombre** a la valeur TRUE.|TRUE si et seulement s’il existe un nœud dans l’ensemble de nœuds tels que lorsque converti en **chaîne**, sa comparaison avec le **chaîne** a la valeur TRUE.|  
|Un est un élément node-set, l’autre un **nombre**.|TRUE si et seulement s’il existe un nœud dans l’ensemble de nœuds tels que lorsque converti en **nombre**, sa comparaison avec le **nombre** a la valeur TRUE.|Idem.|  
|Un est un élément node-set, l’autre un **booléenne**.|TRUE si et seulement s’il existe un nœud dans l’ensemble de nœuds tels que lorsque converti en **booléenne** puis **nombre**, sa comparaison avec le **booléenne** converti en **nombre** a la valeur TRUE.|TRUE si et seulement s’il existe un nœud dans l’ensemble de nœuds tels que lorsque converti en **booléenne**, sa comparaison avec le **booléenne** a la valeur TRUE.|  
|Ni l'un ni l'autre n'est un élément node-set.|Convertir les deux opérandes de **numéro** , puis comparez-les.|Convertissez les deux opérandes en un type commun, puis comparez-les. Convertir en **booléenne** si une est **booléenne**, **nombre** si une est **nombre**; sinon, convertir en **chaîne**.|  
  
> [!NOTE]  
>  Étant donné que les opérateurs relationnels XPath convertissent toujours leurs opérandes en **nombre**, **chaîne** les comparaisons ne sont pas possibles. Pour inclure des comparaisons de dates, SQL Server 2000 propose cette variation à la spécification XPath : lorsqu’un opérateur relationnel compare une **chaîne** à un **chaîne**, un node-set à un **chaîne**, ou un valeur de chaîne node-set à un valeur de chaîne élément node-set, un **chaîne** comparaison (pas un **nombre** comparaison) est effectuée.  
  
## <a name="node-set-conversions"></a>Conversions des éléments node-set  
 Les conversions des éléments node-set ne sont pas toujours intuitives. Un élément node-set est converti en un **chaîne** en prenant la valeur de chaîne du premier nœud dans le jeu. Un élément node-set est converti en **nombre** en le convertissant à **chaîne**, puis convertissez **chaîne** à **nombre**. Un élément node-set est converti en **booléenne** en vérifiant son existence.  
  
> [!NOTE]  
>  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne procède à aucune sélection positionnelle sur les éléments node-set : par exemple, la requête XPath `Customer[3]` désigne le troisième client ; ce type de sélection positionnelle n'est pas pris en charge dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Par conséquent, le nœud-défini-à-**chaîne** ou nœud-défini-à-**nombre** conversions comme décrit dans la spécification XPath ne sont pas implémentées. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise une sémantique « quelconque » partout où la recommandation XPath spécifie la « première » sémantique. Par exemple, en fonction de la spécification XPath du W3C, la requête XPath `Order[OrderDetail/@UnitPrice > 10.0]` sélectionne les commandes dont le premier **OrderDetail** qui a un **UnitPrice** supérieure à 10.0. Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], cette requête XPath sélectionne les commandes avec les **OrderDetail** qui a un **UnitPrice** supérieure à 10.0.  
  
 La conversion en **booléenne** génère une existence test ; par conséquent, la requête XPath `Products[@Discontinued=true()]` est équivalente à l’expression SQL « Products.Discontinued n’est pas null », pas l’expression SQL « Products.Discontinued = 1 ». Pour rendre la requête équivaut à la dernière expression SQL, convertissez d’abord l’élément node-set en non -**booléenne** type, tel que **nombre**. Par exemple, `Products[number(@Discontinued) = true()]`.  
  
 Du fait que la plupart des opérateurs sont définis pour être vrais (TRUE) s'ils le sont pour un nœud quelconque ou l'un des nœuds de l'élément node-set, ces opérations prennent toujours la valeur FALSE si l'élément node-set est vide. Ainsi donc, si A est vide, `A = B` et `A != B` ont tous les deux la valeur FALSE et `not(A=B)` et `not(A!=B)` ont la valeur TRUE.  
  
 En règle générale, un attribut ou un élément qui est mappé à une colonne existe si la valeur de cette colonne dans la base de données n’est pas **null**. Les éléments mappés aux lignes existent si tous leurs enfants existent.  
  
> [!NOTE]  
>  Éléments annotés avec **constante est** existe toujours. Par conséquent, les prédicats XPath ne peut pas être utilisés sur **constante est** éléments.  
  
 Lorsqu’un élément node-set est converti en **chaîne** ou **nombre**, son type XDR (le cas échéant) est examiné dans le schéma annoté et ce type est utilisé pour déterminer la conversion est requise.  
  
## <a name="mapping-xdr-data-types-to-xpath-data-types"></a>Mappage des types de données XDR et XPath  
 Le type de données XPath d’un nœud est dérivé du type de données XDR dans le schéma, comme indiqué dans le tableau suivant (le nœud **EmployeeID** sert à des fins d’illustration).  
  
|Type de données XDR|Équivalent<br /><br /> Type de données XPath|Conversion SQL Server utilisée|  
|-------------------|------------------------------------|--------------------------------|  
|Nonebin.base64bin.hex|Néant|NoneEmployeeID|  
|boolean|boolean|CONVERT(bit, EmployeeID)|  
|number, int, float,i1, i2, i4, i8,r4, r8ui1, ui2, ui4, ui8|nombre|CONVERT(float(53), EmployeeID)|  
|id, idref, idrefsentity, entities, enumerationnotation, nmtoken, nmtokens, chardate, Timedate, Time.tz, string, uri, uuid|chaîne|CONVERT(nvarchar(4000), EmployeeID, 126)|  
|fixed14.4|N/A (aucun type de données XPath n'équivaut au type de données XDR fixed14.4)|CONVERT(money, EmployeeID)|  
|date|chaîne|LEFT(CONVERT(nvarchar(4000), EmployeeID, 126), 10)|  
|time<br /><br /> time.tz|chaîne|SUBSTRING(CONVERT(nvarchar(4000), EmployeeID, 126), 1 + CHARINDEX(N'T', CONVERT(nvarchar(4000), EmployeeID, 126)), 24)|  
  
 Les conversions de date et d’heure sont conçues pour fonctionner si la valeur est stockée dans la base de données à l’aide de la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **datetime** type de données ou un **chaîne**. Notez que la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **datetime** type de données n’utilise pas **fuseau horaire** et a une précision moins importante que le code XML **temps** type de données. Pour inclure la **fuseau horaire** type de données ou une précision supplémentaire, stocker les données dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à l’aide un **chaîne** type.  
  
 Lorsque vous convertissez un nœud du type de données XDR en type de données XPath, une conversion supplémentaire est parfois nécessaire (d'un type de données XPath vers un autre type de données XPath). Par exemple, imaginez la requête XPath suivante :  
  
```  
(@m + 3) = 4  
```  
  
 Si @m est de le **fixed14.4** type de données XDR, la conversion de type de données XDR en type de données s’effectue à l’aide de XPath :  
  
```  
CONVERT(money, m)  
```  
  
 Dans cette conversion, le nœud `m` est converti en **fixed14.4** à **money**. Toutefois, l'ajout de la valeur 3 nécessite une autre conversion :  
  
```  
CONVERT(float(CONVERT(money, m))  
```  
  
 L'expression XPath est évaluée comme suit :  
  
```  
CONVERT(float(CONVERT(money, m)) + CONVERT(float(53), 3) = CONVERT(float(53), 3)  
```  
  
 Comme le montre le tableau ci-dessous, il s'agit de la même conversion que celle appliquée à d'autres expressions XPath (telles que des littéraux ou des expressions composées).  
  
||||||  
|-|-|-|-|-|  
||X est inconnu|X est **chaîne**|X est **nombre**|X est **booléenne**|  
|string(X)|CONVERT (nvarchar(4000), X, 126)|-|CONVERT (nvarchar(4000), X, 126)|CASE WHEN X THEN N'true' ELSE N'false' END|  
|number(X)|CONVERT (float(53), X)|CONVERT (float(53), X)|-|CASE WHEN X THEN 1 ELSE 0 END|  
|boolean(X)|-|LEN(X) > 0|X != 0|-|  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-convert-a-data-type-in-an-xpath-query"></a>A. Convertir un type de données dans une requête XPath  
 Dans la requête XPath suivante spécifiée par rapport à un schéma XSD annoté, la requête sélectionne tous les **employé** nœuds avec le **EmployeeID** la valeur de E-1, où « E- » est le préfixe spécifié à l’aide de l’attribut le **SQL : ID-préfixe** annotation.  
  
 `Employee[@EmployeeID="E-1"]`  
  
 Le prédicat dans la requête équivaut à l'expression SQL suivante :  
  
 `N'E-' + CONVERT(nvarchar(4000), Employees.EmployeeID, 126) = N'E-1'`  
  
 Étant donné que **EmployeeID** est un de la **id** (**idref**, **idrefs**, **nmtoken**, **nmtokens**, et ainsi de suite) les valeurs de type de données dans le schéma XSD, **EmployeeID** est converti en la **chaîne** type de données XPath à l’aide des règles de conversion décrites précédemment.  
  
 `CONVERT(nvarchar(4000), Employees.EmployeeID, 126)`  
  
 Le préfixe  « E - » est ajouté à la chaîne et le résultat est ensuite comparé avec `N'E-1'`.  
  
### <a name="b-perform-several-data-type-conversions-in-an-xpath-query"></a>B. Effectuer plusieurs conversions de types de données dans une requête XPath.  
 Examinez la requête XPath suivante définie par rapport à un schéma XSD annoté : `OrderDetail[@UnitPrice * @OrderQty > 98]`  
  
 Cette requête XPath retourne tous les  **\<OrderDetail >** éléments satisfaisant le prédicat `@UnitPrice * @OrderQty > 98`. Si le **UnitPrice** est annoté avec un **fixed14.4** de type de données dans le schéma annoté, ce prédicat est équivalent à l’expression SQL :  
  
 `CONVERT(float(53), CONVERT(money, OrderDetail.UnitPrice)) * CONVERT(float(53), OrderDetail.OrderQty) > CONVERT(float(53), 98)`  
  
 Lors de la conversion des valeurs dans la requête XPath, la première conversion convertit le type de données XDR en type de données XPath. Étant donné que le type de données XSD de **UnitPrice** est **fixed14.4**, comme décrit dans le tableau précédent, il s’agit de la première conversion qui est utilisée :  
  
```  
CONVERT(money, OrderDetail.UnitPrice))   
```  
  
 Étant donné que les opérateurs arithmétiques convertissent leurs opérandes en le **nombre** type de données XPath, la deuxième conversion (d’un type de données XPath à un autre type de données XPath) est appliquée dans lequel la valeur est convertie en **float (53)** (**float (53)** est proche de l’expression XPath **nombre** type de données) :  
  
```  
CONVERT(float(53), CONVERT(money, OrderDetail.UnitPrice))   
```  
  
 En supposant que le **OrderQty** attribut n’a aucun type de données XSD, **OrderQty** est converti en un **nombre** le type de données XPath dans une conversion unique :  
  
```  
CONVERT(float(53), OrderDetail.OrderQty)  
```  
  
 De même, la valeur 98 est convertie en la **nombre** type de données XPath :  
  
```  
CONVERT(float(53), 98)  
```  
  
> [!NOTE]  
>  Si le type de données XSD appliqué au schéma est incompatible avec le type de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sous-jacent dans la base de données, ou si une conversion de type de données XPath impossible est réalisée, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut retourner une erreur. Par exemple, si le **EmployeeID** attribut est annoté avec **id-prefix** annotation, le XPath `Employee[@EmployeeID=1]` génère une erreur, car **EmployeeID** a le **id-prefix** annotation et ne peut pas être converti en **nombre**.  
  
  
