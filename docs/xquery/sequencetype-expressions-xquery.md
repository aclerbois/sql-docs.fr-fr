---
title: Expressions SequenceType (XQuery) | Documents Microsoft
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
- SequenceType expressions
- instance of operator [XQuery]
- expressions [XQuery], SequenceType
- cast as operator
ms.assetid: ad3573da-d820-4d1c-81c4-a83c4640ce22
caps.latest.revision: 25
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: e2be9250c77c23e00a302d57f0148507f0db41bb
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sequencetype-expressions-xquery"></a>Expressions  SequenceType (XQuery)
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

  Dans XQuery, une valeur est toujours une séquence. Le type de la valeur est désigné par le terme « type de séquence ». Le type de séquence peut être utilisé dans une **instance de** expression XQuery. Vous utilisez la syntaxe SequenceType décrite dans la spécification XQuery lorsque vous devez faire référence à un type dans une expression XQuery.  
  
 Le nom de type atomique peut également être utilisé dans le **castés en tant que** expression XQuery. Dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], le **instance de** et **castés en tant que** expressions XQuery sur des expressions Sequencetype sont partiellement supportées.  
  
## <a name="instance-of-operator"></a>Opérateur instance of  
 Le **instance de** opérateur peut être utilisé pour déterminer le type dynamique, ou d’exécution, de la valeur de l’expression spécifiée. Par exemple :  
  
```  
  
Expression instance of SequenceType[Occurrence indicator]  
```  
  
 Notez que la `instance of` (opérateur), le `Occurrence indicator`, spécifie la cardinalité, le nombre d’éléments dans la séquence résultante. Si cela n'est pas spécifié, il est supposé que la cardinalité est de 1. Dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], uniquement le point d’interrogation (**?)**  indicateur d’occurrence est pris en charge. Le **?** indicateur d’occurrence indique que `Expression` peut retourner zéro ou un élément. Si le **?** indicateur d’occurrence est spécifié, `instance of` retourne True lorsque la `Expression` type corresponde à celui spécifié `SequenceType`, indépendamment de si `Expression` retourne un singleton ou une séquence vide.  
  
 Si le **?** indicateur d’occurrence n’est pas spécifié, `sequence of` renvoie True uniquement lorsque le `Expression` type correspond la `Type` spécifié et `Expression` retourne un singleton.  
  
 **Remarque** le signe plus (**+**) et l’astérisque (**\***) indicateurs d’occurrence ne sont pas pris en charge dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].  
  
 Les exemples suivants illustrent l’utilisation de la**instance de** opérateur XQuery.  
  
### <a name="example-a"></a>Exemple A  
 L’exemple suivant crée un **xml** variable de type et spécifie une requête par rapport à elle. L'expression de requête spécifie un opérateur `instance of` pour déterminer si le type dynamique de la valeur renvoyée par le premier opérande correspond au type spécifié dans le second opérande.  
  
 La requête suivante retourne la valeur True, car la valeur 125 est une instance du type spécifié, **xs : Integer**:  
  
```  
declare @x xml  
set @x=''  
select @x.query('125 instance of xs:integer')  
go  
```  
  
 La requête suivante renvoie True, car la valeur renvoyée par l'expression, /a[1], dans le premier opérande est un élément :  
  
```  
declare @x xml  
set @x='<a>1</a>'  
select @x.query('/a[1] instance of element()')  
go  
```  
  
 De même, `instance of` renvoie True dans la requête suivante, car le type de valeur de l'expression dans la première expression est un attribut :  
  
```  
declare @x xml  
set @x='<a attr1="x">1</a>'  
select @x.query('/a[1]/@attr1 instance of attribute()')  
go  
```  
  
 Dans l'exemple suivant, l'expression `data(/a[1]` renvoie une valeur atomique typée sous la forme xdt:untypedAtomic. Par conséquent, l'opérateur `instance of` renvoie True.  
  
```  
declare @x xml  
set @x='<a>1</a>'  
select @x.query('data(/a[1]) instance of xdt:untypedAtomic')  
go  
```  
  
 Dans la requête suivante, l'expression `data(/a[1]/@attrA` renvoie une valeur atomique non typée. Par conséquent, l'opérateur `instance of` renvoie True.  
  
```  
declare @x xml  
set @x='<a attrA="X">1</a>'  
select @x.query('data(/a[1]/@attrA) instance of xdt:untypedAtomic')  
go  
```  
  
### <a name="example-b"></a>Exemple B  
 Dans cet exemple, vous interrogez une colonne XML typée de l'exemple de base de données AdventureWorks. La collection de schémas XML associée à la colonne interrogée fournit les informations de définition de type.  
  
 Dans l’expression, **data()** retourne la valeur typée de l’attribut ProductModelID dont le type est xs : String en fonction du schéma associé à la colonne. Par conséquent, l'opérateur `instance of` renvoie True.  
  
```  
SELECT CatalogDescription.query('  
   declare namespace PD="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
   data(/PD:ProductDescription[1]/@ProductModelID) instance of xs:string  
') as Result  
FROM Production.ProductModel  
WHERE ProductModelID = 19  
```  
  
 Pour plus d’informations, consultez [Comparer du XML typé et du XML non typé](../relational-databases/xml/compare-typed-xml-to-untyped-xml.md).  
  
 Les requêtes suivantes usetheBoolean `instance of` expression pour déterminer si l’attribut LocationID est de type xs : Integer :  
  
```  
SELECT Instructions.query('  
   declare namespace AWMI="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelManuInstructions";  
   /AWMI:root[1]/AWMI:Location[1]/@LocationID instance of attribute(LocationID,xs:integer)  
') as Result  
FROM Production.ProductModel  
WHERE ProductModelID=7  
```  
  
 La requête suivante porte sur la colonne XML typée CatalogDescription. La collection de schémas XML associée à cette colonne fournit les informations de définition de type.  
  
 La requête utilise le test `element(ElementName, ElementType?)` dans l'expression `instance of` pour vérifier que `/PD:ProductDescription[1]` renvoie un nœud d'élément d'un nom et d'un type spécifiques.  
  
```  
SELECT CatalogDescription.query('  
     declare namespace PD="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
     /PD:ProductDescription[1] instance of element(PD:ProductDescription, PD:ProductDescription?)  
    ') as Result  
FROM  Production.ProductModel  
where ProductModelID=19  
```  
  
 La requête renvoie la valeur True.  
  
### <a name="example-c"></a>Exemple C  
 Lors de l'utilisation de types d'union, l'expression `instance of` dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] présente une limite : en particulier, lorsque le type d'un élément ou d'un attribut est un type d'union, `instance of` peut ne pas déterminer le type exact. De ce fait, une requête renvoie False, sauf si le type atomique utilisé dans le type de séquence est le parent le plus élevé du type réel de l'expression dans la hiérarchie simpleType. En d'autres termes, les types atomiques spécifiés dans le type de séquence doivent être un enfant direct de anySimpleType. Pour plus d’informations sur la hiérarchie des types, consultez [règles de conversion de Type dans XQuery](../xquery/type-casting-rules-in-xquery.md).  
  
 L'exemple de requête ci-après effectue les opérations suivantes :  
  
-   Créer une collection de schémas XML dans laquelle est défini un type d'union, tel qu'un type integer ou string.  
  
-   Déclarez un typé **xml** variable à l’aide de la collection de schémas XML.  
  
-   Affecter un exemple d'instance XML à la variable.  
  
-   Interroger la variable pour illustrer le comportement de l'opérateur `instance of` lors de l'utilisation d'un type d'union.  
  
 Voici la requête :  
  
```  
CREATE XML SCHEMA COLLECTION MyTestSchema AS '  
<schema xmlns="http://www.w3.org/2001/XMLSchema" targetNamespace="http://ns" xmlns:ns="http://ns">  
<simpleType name="MyUnionType">  
<union memberTypes="integer string"/>  
</simpleType>  
<element name="TestElement" type="ns:MyUnionType"/>  
</schema>'  
Go  
```  
  
 La requête suivante renvoie False, car le type de séquence spécifié dans l'expression `instance of` n'est pas le parent le plus élevé du type réel de l'expression spécifiée. En d'autres termes, la valeur de <`TestElement`> est un type integer. Le parent le plus élevé est xs:decimal. Toutefois, il n'est pas spécifié comme second opérande de l'opérateur `instance of`.  
  
```  
SET QUOTED_IDENTIFIER ON  
DECLARE @var XML(MyTestSchema)  
  
SET @var = '<TestElement xmlns="http://ns">123</TestElement>'  
  
SELECT @var.query('declare namespace ns="http://ns"   
   data(/ns:TestElement[1]) instance of xs:integer')  
go  
```  
  
 Étant donné que le parent le plus élevé de xs:integer est xs:decimal, la requête renvoie True si vous la modifiez et que vous y spécifiez xs:decimal comme type de séquence.  
  
```  
SET QUOTED_IDENTIFIER ON  
DECLARE @var XML(MyTestSchema)  
SET @var = '<TestElement xmlns="http://ns">123</TestElement>'  
SELECT @var.query('declare namespace ns="http://ns"     
   data(/ns:TestElement[1]) instance of xs:decimal')  
go  
```  
  
### <a name="example-d"></a>Exemple D  
 Dans cet exemple, vous tout d’abord créer une collection de schémas XML et l’utiliser pour taper une **xml** variable. Typées **xml** variable est ensuite interrogée pour illustrer la `instance of` fonctionnalité.  
  
 La collection de schémas XML suivante définit un type simple, myType, et un élément, <`root`>, de type myType :  
  
```  
drop xml schema collection SC  
go  
CREATE XML SCHEMA COLLECTION SC AS '  
<schema xmlns="http://www.w3.org/2001/XMLSchema" targetNamespace="myNS" xmlns:ns="myNS"  
xmlns:s="http://schemas.microsoft.com/sqlserver/2004/sqltypes">  
      <import namespace="http://schemas.microsoft.com/sqlserver/2004/sqltypes"/>  
      <simpleType name="myType">  
           <restriction base="s:varchar">  
                  <maxLength value="20"/>  
            </restriction>  
      </simpleType>  
      <element name="root" type="ns:myType"/>  
</schema>'  
Go  
```  
  
 Créer maintenant typé **xml** variable et les interroger :  
  
```  
DECLARE @var XML(SC)  
SET @var = '<root xmlns="myNS">My data</root>'  
SELECT @var.query('declare namespace sqltypes = "http://schemas.microsoft.com/sqlserver/2004/sqltypes";  
declare namespace ns="myNS";   
   data(/ns:root[1]) instance of ns:myType')  
go  
```  
  
 Étant donné que le type myType dérive par restriction d'un type varchar défini dans le schéma sqltypes, `instance of` renvoie également True.  
  
```  
DECLARE @var XML(SC)  
SET @var = '<root xmlns="myNS">My data</root>'  
SELECT @var.query('declare namespace sqltypes = "http://schemas.microsoft.com/sqlserver/2004/sqltypes";  
declare namespace ns="myNS";   
data(/ns:root[1]) instance of sqltypes:varchar?')  
go  
```  
  
### <a name="example-e"></a>Exemple E  
 Dans l'exemple suivant, l'expression extrait l'une des valeurs de l'attribut IDREFS et utilise l'opérateur `instance of` pour déterminer si la valeur est de type IDREF. Cet exemple illustre les opérations suivantes :  
  
-   Crée une collection de schémas XML dans laquelle la <`Customer`> élément a un **OrderList** attribut de type IDREFS et la <`Order`> élément a un **OrderID** attribut de type ID.  
  
-   Crée un typé **xml** variable et lui affecte un exemple de code XML d’instance.  
  
-   Spécifier une requête par rapport à la variable. L'expression de requête extrait la première valeur d'ID de commande de l'attribut OrderList de type IDRERS du premier <`Customer`>. La valeur extraite est de type IDREF. Par conséquent, `instance of` retourne la valeur True.  
  
```  
create xml schema collection SC as  
'<schema xmlns="http://www.w3.org/2001/XMLSchema" xmlns:Customers="Customers" targetNamespace="Customers">  
            <element name="Customers" type="Customers:CustomersType"/>  
            <complexType name="CustomersType">  
                        <sequence>  
                            <element name="Customer" type="Customers:CustomerType" minOccurs="0" maxOccurs="unbounded" />  
                        </sequence>  
            </complexType>  
             <complexType name="OrderType">  
                <sequence minOccurs="0" maxOccurs="unbounded">  
                            <choice>  
                                <element name="OrderValue" type="integer" minOccurs="0" maxOccurs="unbounded"/>  
                            </choice>  
                </sequence>                                             
                <attribute name="OrderID" type="ID" />  
            </complexType>  
  
            <complexType name="CustomerType">  
                <sequence minOccurs="0" maxOccurs="unbounded">  
                            <choice>  
                                <element name="spouse" type="string" minOccurs="0" maxOccurs="unbounded"/>  
                                <element name="Order" type="Customers:OrderType" minOccurs="0" maxOccurs="unbounded"/>  
                            </choice>  
                </sequence>                                             
                <attribute name="CustomerID" type="string" />  
                <attribute name="OrderList" type="IDREFS" />  
            </complexType>  
 </schema>'  
go  
declare @x xml(SC)  
set @x='<CustOrders:Customers xmlns:CustOrders="Customers">  
                <Customer CustomerID="C1" OrderList="OrderA OrderB"  >  
                              <spouse>Jenny</spouse>  
                                <Order OrderID="OrderA"><OrderValue>11</OrderValue></Order>  
                                <Order OrderID="OrderB"><OrderValue>22</OrderValue></Order>  
  
                </Customer>  
                <Customer CustomerID="C2" OrderList="OrderC OrderD" >  
                                <spouse>John</spouse>  
                                <Order OrderID="OrderC"><OrderValue>33</OrderValue></Order>  
                                <Order OrderID="OrderD"><OrderValue>44</OrderValue></Order>  
  
                        </Customer>  
                <Customer CustomerID="C3"  OrderList="OrderE OrderF" >  
                                <spouse>Jane</spouse>  
                                <Order OrderID="OrderE"><OrderValue>55</OrderValue></Order>  
                                <Order OrderID="OrderF"><OrderValue>55</OrderValue></Order>  
                </Customer>  
                <Customer CustomerID="C4"  OrderList="OrderG"  >  
                                <spouse>Tim</spouse>  
                                <Order OrderID="OrderG"><OrderValue>66</OrderValue></Order>  
                        </Customer>  
                <Customer CustomerID="C5"  >  
                </Customer>  
                <Customer CustomerID="C6" >  
                </Customer>  
                <Customer CustomerID="C7"  >  
                </Customer>  
</CustOrders:Customers>'  
  
select @x.query(' declare namespace CustOrders="Customers";   
 data(CustOrders:Customers/Customer[1]/@OrderList)[1] instance of xs:IDREF ? ') as XML_result  
```  
  
### <a name="implementation-limitations"></a>Limites de mise en œuvre  
 Les limitations suivantes s'appliquent :  
  
-   Le **schema-element()** et **schema-attribute()** types de séquence ne sont pas pris en charge pour une comparaison avec le `instance of` opérateur.  
  
-   Les séquences complètes, telles que `(1,2) instance of xs:integer*`, ne sont pas prises en charge.  
  
-   Lorsque vous utilisez une forme de la **element()** de séquence de type qui spécifie un nom de type, tel que `element(ElementName, TypeName)`, le type doit être qualifié avec un point d’interrogation ( ?). Par exemple, `element(Title, xs:string?)` indique que l'élément peut être NULL. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] ne prend pas en charge la détection de l’exécution de la **xsi : nil** propriété à l’aide de `instance of`.  
  
-   Si la valeur de `Expression` provient d'un élément ou d'un attribut typé en tant qu'union, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] peut uniquement identifier le type de primitive, non dérivé, duquel le type de la valeur a été dérivé. Par exemple, si <`e1`> est défini de manière à posséder le type statique (xs:integer | xs:string), la syntaxe ci-après renvoie False.  
  
    ```  
    data(<e1>123</e1>) instance of xs:integer  
    ```  
  
     Toutefois, `data(<e1>123</e1>) instance of xs:decimal` renvoie True.  
  
-   Pour le **//processing-instruction ()** et **document-node()** les types de séquence, seules les formes sans arguments sont autorisés. Par exemple, `processing-instruction()` est autorisée, mais `processing-instruction('abc')` n’est pas autorisée.  
  
## <a name="cast-as-operator"></a>Opérateur cast as  
 Le **castés en tant que** expression peut être utilisée pour convertir une valeur en un type de données spécifique. Par exemple :  
  
```  
  
Expression cast as  AtomicType?  
```  
  
 Dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], le point d'interrogation (?) est requis après `AtomicType`. Par exemple, comme indiqué dans la requête suivante, `"2" cast as xs:integer?` convertit la valeur de chaîne en un entier :  
  
```  
declare @x xml  
set @x=''  
select @x.query('"2" cast as xs:integer?')  
```  
  
 Dans la requête suivante, **data()** retourne la valeur typée de l’attribut ProductModelID, un type chaîne. Le `cast as`opérateur convertit la valeur en xs : Integer.  
  
```  
WITH XMLNAMESPACES ('http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription' AS PD)  
SELECT CatalogDescription.query('  
   data(/PD:ProductDescription[1]/@ProductModelID) cast as xs:integer?  
') as Result  
FROM Production.ProductModel  
WHERE ProductModelID = 19  
```  
  
 L’utilisation explicite de **data()** n’est pas requise dans cette requête. L'expression `cast as` effectue une atomisation implicite sur l'expression d'entrée.  
  
### <a name="constructor-functions"></a>Fonctions constructeur  
 Vous pouvez utiliser les fonctions constructeur de type atomique. Par exemple, au lieu d’utiliser le `cast as` (opérateur), `"2" cast as xs:integer?`, vous pouvez utiliser la **xs:integer()** fonction constructeur, comme dans l’exemple suivant :  
  
```  
declare @x xml  
set @x=''  
select @x.query('xs:integer("2")')  
```  
  
 L'exemple suivant renvoie une valeur xs:date égale à 2000-01-01Z.  
  
```  
declare @x xml  
set @x=''  
select @x.query('xs:date("2000-01-01Z")')  
```  
  
 Vous pouvez également utiliser des constructeurs pour les types atomiques définis par l'utilisateur. Par exemple, si la collection de schémas XML associé avec le type de données XML définit un type simple, un **myType()** constructeur peut être utilisé pour retourner une valeur de ce type.  
  
#### <a name="implementation-limitations"></a>Limites de mise en œuvre  
  
-   Les expressions XQuery **typeswitch**, **convertibles**, et **traiter** ne sont pas pris en charge.  
  
-   **castés en tant que** requiert un point d’interrogation ( ?) après le type atomique.  
  
-   **xs : QName** n’est pas pris en charge en tant que type pour la conversion. Utilisez **expanded-QName** à la place.  
  
-   **xs : date**, **xs : Time**, et **xs : DateTime** requièrent un fuseau horaire, qui est indiqué par un Z.  
  
     La requête suivante échoue, car le fuseau horaire n'est pas spécifié.  
  
    ```  
    DECLARE @var XML  
    SET @var = ''  
    SELECT @var.query(' <a>{xs:date("2002-05-25")}</a>')  
    go  
    ```  
  
     Si vous ajoutez l'indicateur de fuseau horaire Z à la valeur, la requête fonctionne.  
  
    ```  
    DECLARE @var XML  
    SET @var = ''  
    SELECT @var.query(' <a>{xs:date("2002-05-25Z")}</a>')  
    go  
    ```  
  
     Voici le résultat obtenu :  
  
    ```  
    <a>2002-05-25Z</a>  
    ```  
  
## <a name="see-also"></a>Voir aussi  
 [Expressions XQuery](../xquery/xquery-expressions.md)   
 [Système de type &#40;XQuery&#41;](../xquery/type-system-xquery.md)  
  
  
