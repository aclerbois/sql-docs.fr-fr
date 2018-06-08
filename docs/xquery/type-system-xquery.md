---
title: Type de système (XQuery) | Documents Microsoft
ms.custom: ''
ms.date: 08/10/2016
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
- type system [XQuery]
- typed values [XQuery]
- XQuery, sequence
- string values [XQuery]
- XQuery, XPath data type namespace
- xdt prefix [XML in SQL Server]
- XQuery, type system
- built-in XML schema types [SQL Server]
- xs prefix [XML in SQL Server]
ms.assetid: 22d6f861-d058-47ee-b550-cbe9092dcb12
caps.latest.revision: 28
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: dcdaf7607d425dec01a9f2cf8c87ff55e5441aae
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="type-system-xquery"></a>Système de types (XQuery)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  XQuery est un langage fortement typé pour les types de schéma et un langage faiblement typé pour les données non typées. Les types prédéfinis de XQuery incluent les suivants :  
  
-   Types intégrés de schéma XML dans le **http://www.w3.org/2001/XMLSchema** espace de noms.  
  
-   Types définis dans le **http://www.w3.org/2004/07/xpath-datatypes** espace de noms.  
  
 Cette rubrique décrit également les éléments suivants :  
  
-   La valeur typée par rapport à la valeur de chaîne d'un nœud.  
  
-   Le [fonction data &#40;XQuery&#41; ](../xquery/data-accessor-functions-data-xquery.md) et [fonction string &#40;XQuery&#41;](../xquery/data-accessor-functions-string-xquery.md).  
  
-   Mise en correspondance du type de séquence retourné par une expression.  
  
## <a name="built-in-types-of-xml-schema"></a>Types intégrés de schéma XML  
 Les types intégrés de schéma XML possèdent un préfixe d'espace de noms prédéfini, xs. Certains de ces types incluent **xs : Integer** et **xs : String**. Tous ces types intégrés sont pris en charge. Vous pouvez utiliser ces types lorsque vous créez une collection de schémas XML.  
  
 Lorsque vous interrogez des données XML typées, le type statique et dynamique des nœuds est déterminé par la collection de schémas XML associée à la colonne ou à la variable sur laquelle porte la requête. Pour plus d’informations sur les types statiques et dynamiques, consultez [contexte de l’Expression et d’évaluation de la requête &#40;XQuery&#41;](../xquery/expression-context-and-query-evaluation-xquery.md). Par exemple, la requête suivante est spécifiée contre un typé **xml** colonne (`Instructions`). L'expression utilise `instance of` pour vérifier que la valeur typée de l'attribut `LotSize` retourné est de type `xs:decimal`.  
  
```  
SELECT Instructions.query('  
   DECLARE namespace AWMI="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
   data(/AWMI:root[1]/AWMI:Location[@LocationID=10][1]/@LotSize)[1] instance of xs:decimal  
') AS Result  
FROM Production.ProductModel  
WHERE ProductModelID=7  
```  
  
 Cette information de type est fournie par la collection de schémas XML associée à la colonne.  
  
## <a name="types-defined-in-xpath-data-types-namespace"></a>Types définis dans l'espace de noms des types de données XPath  
 Les types définis dans le **http://www.w3.org/2004/07/xpath-datatypes** ont le préfixe prédéfini d’espace de noms **xdt**. Les règles suivantes s'appliquent à ces types :  
  
-   Vous ne pouvez pas utiliser ces types lorsque vous créez une collection de schémas XML. Ces types sont utilisés dans le système de type XQuery et sont utilisés pour [XQuery et le typage statique](../xquery/xquery-and-static-typing.md). Vous pouvez effectuer un cast vers les types atomiques, par exemple, **xdt : untypedAtomic**, dans le **xdt** espace de noms.  
  
-   Lors de l’interrogation des données XML non typées, le type statique et dynamique des nœuds d’élément est **xdt : non typé**, et le type de valeurs d’attribut est **xdt : untypedAtomic**. Le résultat d’une **query()** méthode génère le code XML non typé. Cela signifie que les nœuds XML sont retournés en tant que **xdt : non typé** et **xdt : untypedAtomic**, respectivement.  
  
-   Le **xdt : daytimeduration** et **xdt : yearmonthduration** types ne sont pas pris en charge.  
  
 Dans l'exemple suivant, la requête est spécifiée sur une variable de type XML non typé. L'expression, `data(/a[1]`), retourne une séquence d'une valeur atomique. La fonction `data()` retourne la valeur typée de l'élément `<a>`. Comme les données XML interrogées sont non typées, le type de la valeur retournée est `xdt:untypedAtomic`. Par conséquent, `instance of` retourne true.  
  
```  
DECLARE @x xml  
SET @x='<a>20</a>'  
SELECT @x.query( 'data(/a[1]) instance of xdt:untypedAtomic' )  
```  
  
 Au lieu de récupérer la valeur typée, l'expression (`/a[1]`) dans l'exemple ci-dessous retourne une séquence d'un élément, l'élément `<a>`. L'expression `instance of` utilise le test d'élément pour vérifier que la valeur retournée par l'expression est un nœud d'élément de `xdt:untyped type`.  
  
```  
DECLARE @x xml  
SET @x='<a>20</a>'  
-- Is this an element node whose name is "a" and type is xdt:untyped.  
SELECT @x.query( '/a[1] instance of element(a, xdt:untyped?)')  
-- Is this an element node of type xdt:untyped.  
SELECT @x.query( '/a[1] instance of element(*, xdt:untyped?)')  
-- Is this an element node?  
SELECT @x.query( '/a[1] instance of element()')  
```  
  
> [!NOTE]  
>  Lorsque vous interrogez une instance XML typée et que l'expression de requête inclut l'axe parent, le type statique d'information des nœuds résultants n'est plus disponible. Toutefois, le type dynamique est encore associé aux nœuds.  
  
## <a name="typed-value-vs-string-value"></a>Valeur typée par rapport à valeur de chaîne  
 Chaque nœud possède une valeur typée et une valeur de chaîne. Pour les données XML typées, le type de la valeur typée est fourni par la collection de schémas XML associée à la colonne ou à la variable sur laquelle porte la requête. Pour les données XML non typées, le type de la valeur typée est **xdt : untypedAtomic**.  
  
 Vous pouvez utiliser la **data()** ou **string()** fonction pour récupérer la valeur d’un nœud :  
  
-   Le [fonction data &#40;XQuery&#41; ](../xquery/data-accessor-functions-data-xquery.md) retourne la valeur typée d’un nœud.  
  
-   Le [fonction string &#40;XQuery&#41; ](../xquery/data-accessor-functions-string-xquery.md) retourne la valeur de chaîne du nœud.  
  
 Dans la collection de schémas XML ci-dessous, l'élément <`root`> du type entier est défini :  
  
```  
CREATE XML SCHEMA COLLECTION SC AS N'  
<schema xmlns="http://www.w3.org/2001/XMLSchema">  
      <element name="root" type="integer"/>  
</schema>'  
GO  
```  
  
 Dans l'exemple ci-dessous, l'expression commence par récupérer la valeur typée de `/root[1]` et lui ajoute `3`.  
  
```  
DECLARE @x xml(SC)  
SET @x='<root>5</root>'  
SELECT @x.query('data(/root[1]) + 3')  
```  
  
 Dans l'exemple suivant, l'expression échoue, car `string(/root[1])` dans l'expression retourne une valeur de type de chaîne. Cette valeur est alors transmise à un opérateur arithmétique qui accepte uniquement les valeurs de type numérique comme opérandes.  
  
```  
-- Fails because the argument is string type (must be numeric primitive type).  
DECLARE @x xml(SC)  
SET @x='<root>5</root>'  
SELECT @x.query('string(/root[1]) + 3')  
```  
  
 L'exemple ci-dessous calcule le total des attributs `LaborHours`. Le `data()` fonction récupère les valeurs typées des `LaborHours` de tous les attributs de la <`Location`> éléments pour un modèle de produit. Selon le schéma XML associé à la `Instruction` colonne, `LaborHours` est de **xs : decimal** type.  
  
```  
SELECT Instructions.query('   
DECLARE namespace AWMI="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";   
             sum(data(//AWMI:Location/@LaborHours))   
') AS Result   
FROM Production.ProductModel   
WHERE ProductModelID=7  
```  
  
 Cette requête retourne un résultat égal à 12,75.  
  
> [!NOTE]  
>  L’utilisation explicite de la **data()** fonction dans cet exemple est à titre d’illustration uniquement. S’il n’est pas spécifié, **sum()** applique implicitement la **data()** fonction permettant d’extraire les valeurs typées des nœuds.  
  
## <a name="see-also"></a>Voir aussi  
 [Modèles et autorisations du générateur de SQL Server Profiler](../tools/sql-server-profiler/sql-server-profiler-templates-and-permissions.md)   
 [Concepts de base de XQuery](../xquery/xquery-basics.md)  
  
  
