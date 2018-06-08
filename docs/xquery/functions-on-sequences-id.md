---
title: ID de fonction (XQuery) | Documents Microsoft
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
- fn:id function
- id function
ms.assetid: de99fc60-d0ad-4117-a17d-02bdde6512b4
caps.latest.revision: 19
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: b31e1dc2894511d56cf8809396853dbb0a2e8329
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="functions-on-sequences---id"></a>Fonctions sur les séquences - id
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

  Retourne la séquence de nœuds d’éléments avec des valeurs xs : ID qui correspondent aux valeurs d’un ou plusieurs des valeurs xs : IDREF fournies dans *$arg*.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
fn:id($arg as xs:IDREF*) as element()*  
```  
  
## <a name="arguments"></a>Arguments  
 *$arg*  
 Une ou plusieurs valeurs xs:IDREF.  
  
## <a name="remarks"></a>Notes  
 Le résultat de la fonction est une séquence d'éléments de l'instance XML, dans l'ordre du document, qui ont une valeur xs:ID égale à une ou plusieurs des valeurs xs:IDREF de la liste des candidats xs:IDREF.  
  
 Si la valeur xs:IDREF ne correspond à aucun élément, la fonction renvoie la séquence vide.  
  
## <a name="examples"></a>Exemples  
 Cette rubrique fournit des exemples de XQuery relatifs à des instances XML stockés dans différentes **xml** type des colonnes dans le [!INCLUDE[ssSampleDBobject](../includes/sssampledbobject-md.md)] base de données.  
  
### <a name="a-retrieving-elements-based-on-the-idref-attribute-value"></a>A. Récupération des éléments en fonction de la valeur de l'attribut IDREF  
 L'exemple suivant utilise la fonction fn:id pour récupérer les éléments <`employee`>, en fonction de l'attribut IDREF manager. Dans cet exemple, l'attribut manager est de type IDREF et l'attribut eid est de type ID.  
  
 Pour une valeur d’attribut de gestionnaire spécifique, le **id()** fonction recherche le <`employee`> élément dont valeur d’attribut de type ID correspond à la valeur IDREF en entrée. En d’autres termes, pour un employé spécifique, le **id()** fonction retourne le responsable de l’employé.  
  
 Voici ce qui se passe dans l'exemple :  
  
-   Une collection de schémas XML est créée.  
  
-   Typé **xml** variable est créée à l’aide de la collection de schémas XML.  
  
-   La requête récupère l’élément qui a une valeur d’attribut ID référencée par la **manager** attribut IDREF de la <`employee`> élément.  
  
```  
-- If exists, drop the XML schema collection (SC).  
-- drop xml schema collection SC  
-- go  
  
create xml schema collection SC as  
'<schema xmlns="http://www.w3.org/2001/XMLSchema" xmlns:e="emp" targetNamespace="emp">  
            <element name="employees" type="e:EmployeesType"/>  
            <complexType name="EmployeesType">  
                 <sequence>  
                      <element name="employee" type="e:EmployeeType" minOccurs="0" maxOccurs="unbounded" />  
                 </sequence>  
            </complexType>    
  
            <complexType name="EmployeeType">  
                        <attribute name="eid" type="ID" />  
                        <attribute name="name" type="string" />  
                        <attribute name="manager" type="IDREF" />  
            </complexType>         
</schema>'  
go  
```  
  
```  
declare @x xml(SC)  
set @x='<e:employees xmlns:e="emp">  
<employee eid="e1" name="Joe" manager="e10" />  
<employee eid="e2" name="Bob" manager="e10" />  
<employee eid="e10" name="Dave" manager="e10" />  
</e:employees>'  
  
select @x.value(' declare namespace e="emp";   
 (fn:id(e:employees/employee[@name="Joe"]/@manager)/@name)[1]', 'varchar(50)')   
Go  
```  
  
 La requête renvoie la valeur « Dave », ce qui indique que Dave est le supérieur hiérarchique de Joe.  
  
### <a name="b-retrieving-elements-based-on-the-orderlist-idrefs-attribute-value"></a>B. Récupération des éléments en fonction de la valeur de l'attribut IDREFS OrderList  
 Dans l'exemple suivant, l'attribut OrderList de l'élément <`Customer`> est un attribut de type IDREFS. Il répertorie les ID de commande se rapportant à un client particulier. Pour chaque ID de commande, il existe un enfant de l'élément <`Order`> sous l'élément <`Customer`> fournissant la valeur de la commande.  
  
 L'expression de la requête, `data(CustOrders:Customers/Customer[1]/@OrderList)[1]`, récupère la première valeur de la liste IDREFS pour le premier client. Cette valeur est ensuite passée à la **id()** (fonction). La fonction recherche ensuite la <`Order`> élément dont valeur de l’attribut OrderID correspond à l’entrée de la **id()** (fonction).  
  
```  
drop xml schema collection SC  
go  
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
select @x.query('declare namespace CustOrders="Customers";  
  id(data(CustOrders:Customers/Customer[1]/@OrderList)[1])')  
  
-- result  
<Order OrderID="OrderA">  
  <OrderValue>11</OrderValue>  
</Order>  
```  
  
### <a name="implementation-limitations"></a>Limites de mise en œuvre  
 Les limitations suivantes s'appliquent :  
  
-   [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] ne prend pas en charge la version à deux arguments de **id()**.  
  
-   [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] requiert le type d’argument de **id()** à être un sous-type de xs : IDREF *.  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctions sur les séquences](http://msdn.microsoft.com/library/672d2795-53ab-49c2-bf24-bc81a47ecd3f)  
  
  
