---
title: OPENXML (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/20/2018
ms.prod: sql
ms.prod_service: sql-database
ms.component: t-sql|functions
ms.reviewer: ''
ms.suite: sql
ms.technology: t-sql
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- OPENXML_TSQL
- OPENXML
dev_langs:
- TSQL
helpviewer_keywords:
- OPENXML statement
- rowsets [SQL Server], XML documents
- XML [SQL Server], rowset views
ms.assetid: 8088b114-7d01-435a-8e0d-b81abacc86d6
caps.latest.revision: 24
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: e3ad07906a4b64281016da04ee803833e3d17542
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="openxml-transact-sql"></a>OPENXML (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-asdb-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-asdb-xxxx-xxx-md.md)]

  OPENXML fournit une vue de l'ensemble des lignes d'un document XML. En tant que fournisseur d'ensembles de lignes, OPENXML peut être inclus dans les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] dans lesquelles certains fournisseurs d'ensembles de lignes peuvent apparaître (notamment une table, une vue ou la fonction OPENROWSET).  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
OPENXML( idoc int [ in] , rowpattern nvarchar [ in ] , [ flags byte [ in ] ] )   
[ WITH ( SchemaDeclaration | TableName ) ]  
```  
  
## <a name="arguments"></a>Arguments  
 *idoc*  
 Descripteur de document utilisé pour la représentation interne d'un document XML. La représentation interne d’un document XML est créée en appelant **sp_xml_preparedocument**.  
  
 *rowpattern*  
 Modèle XPath utilisé pour identifier les nœuds (dans le document XML dont le descripteur est transmis dans le paramètre *idoc*) à traiter comme des lignes.  
  
 *flags*  
 Indique le mappage qui doit être utilisé entre les données XML et l'ensemble de lignes relationnel, ainsi que le mode de remplissage de la colonne de débordement. *flags* est un paramètre d’entrée facultatif qui peut prendre l’une des valeurs suivantes.  
  
|Valeur d'octet|Description|  
|----------------|-----------------|  
|**0**|Utilise par défaut le mappage **attribute-centric**.|  
|**1**|Utilise le mappage **attribute-centric**. Peut être combiné avec XML_ELEMENTS. Dans ce cas, le mappage **attribute-centric** est d’abord appliqué, puis le mappage **element-centric** est appliqué pour toutes les colonnes qui n’ont pas encore été traitées.|  
|**2**|Utilisez le mappage **element-centric**. Peut être combiné avec XML_ATTRIBUTES. Dans ce cas, le mappage **attribute-centric** est d’abord appliqué, puis le mappage **element-centric** est appliqué pour toutes les colonnes qui n’ont pas encore été traitées.|  
|**8**|Peut être combiné (à l'aide de l'opérateur logique OR) avec XML_ATTRIBUTES ou XML_ELEMENTS. Dans le contexte d’une extraction, cet indicateur indique que les données consommées ne doivent pas être copiées vers la propriété de dépassement **@mp:xmltext**.|  
  
 *SchemaDeclaration*  
 Définition de schéma de la forme : *ColName**ColType* [*ColPattern* | *MetaProperty*] [**,***ColNameColType* [* ColPattern* | *MetaProperty*]...]  
  
 *ColName*  
 Nom de la colonne dans l'ensemble de lignes.  
  
 *ColType*  
 Type de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de la colonne dans l'ensemble de lignes. Si les types de colonnes diffèrent du type de données **xml** sous-jacent de l’attribut, un forçage de type se produit.  
  
 *ColPattern*  
 Modèle XPath général et optionnel, qui décrit les modalités de mappage des nœuds XML vers les colonnes. Si *ColPattern* n’est pas spécifié, le mappage par défaut (mappage **attribute-centric** ou **element-centric** tel que spécifié par *flags*) est appliqué.  
  
 Le modèle XPath spécifié dans *ColPattern* permet de préciser qu’il s’agit d’un type de mappage particulier (dans le cas d’un mappage **attribute-centric** ou **element-centric**) qui remplace ou améliore le mappage par défaut indiqué par *flags*.  
  
 Le modèle XPath général spécifié comme *ColPattern* prend également en charge les métapropriétés.  
  
 *MetaProperty*  
 L'une des métapropriétés fournies par OPENXML. Si *MetaProperty* est spécifié, la colonne contient les informations fournies par cette métapropriété. Les métapropriétés vous permettent d'extraire des informations (telles que la position relative et les informations sur l'espace de noms) concernant les nœuds XML. Vous disposez ainsi de plus d'informations que n'en propose la représentation textuelle.  
  
 *TableName*  
 Nom de la table qui peut être fourni (à la place de *SchemaDeclaration*) si une table comportant le schéma souhaité existe déjà et qu’aucun modèle de colonne n’est requis.  
  
## <a name="remarks"></a>Notes   
 La clause WITH fournit un format d’ensemble de lignes (et d’autres informations de mappage pertinentes) en utilisant *SchemaDeclaration* ou en spécifiant un *TableName* existant. Si la clause WITH facultative n’est pas spécifiée, les résultats sont retournés dans un format de table **edge**. Les tables edge représentent la structure à granularité fine des documents XML (par exemple, les noms d'élément/d'attribut, la hiérarchie du document, les espaces de noms, les instructions de traitement, etc.) dans une seule table.  
  
 Le tableau suivant décrit la structure de la table **edge**.  
  
|Nom de colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**id**|**bigint**|ID unique du nœud du document.<br /><br /> L’ID de l’élément racine a pour valeur 0. Les valeurs négatives d'ID sont réservées.|  
|**parentid**|**bigint**|Identifie le parent du nœud. Le parent identifié par cet ID n'est pas nécessairement l'élément parent mais il dépend du type du nœud dont le parent est identifié par cet ID. Par exemple, si le nœud est un nœud texte, son parent peut être un nœud d'attribut.<br /><br /> Si le nœud se situe au niveau supérieur du document XML, son **ParentID** a pour valeur NULL.|  
|**nodetype**|**Int**|Identifie le type de nœud. Il s'agit d'un entier qui correspond à la numérotation des types de nœud XML DOM.<br /><br /> Les types de nœuds sont :<br /><br /> 1 = Nœud d'élément<br /><br /> 2 = Nœud d'attribut<br /><br /> 3 = Nœud de texte|  
|**localname**|**nvarchar**|Fournit le nom local de l'élément ou de l'attribut. A pour valeur NULL si l'objet DOM est dépourvu de nom.|  
|**prefix**|**nvarchar**|Préfixe de l'espace de noms du nom de nœud.|  
|**namespaceuri**|**nvarchar**|URI de l'espace de noms du nœud. Si la valeur est NULL, aucun espace de noms n'est présent.|  
|**datatype**|**nvarchar**|Type de données réel de la ligne d'éléments ou d'attributs, sinon NULL. Le type de données est déduit de la DTD en ligne ou du schéma en ligne.|  
|**prev**|**bigint**|ID XML de l'élément frère précédent. Vaut NULL en l'absence de frère précédent direct.|  
|**texte**|**ntext**|Contient la valeur de l’attribut ou le contenu de l’élément sous forme de texte (ou NULL si l’entrée de table **edge** ne requiert pas de valeur).|  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-a-simple-select-statement-with-openxml"></a>A. Utilisation d'une instruction SELECT simple avec OPENXML  
 Cet exemple crée une représentation interne de l'image XML à l'aide de `sp_xml_preparedocument`. Une instruction `SELECT` est alors exécutée sur la représentation interne du document XML, en utilisant un fournisseur d'ensembles de lignes `OPENXML`.  
  
 La valeur *flag* est définie sur `1`. Cela indique un mappage **attribute-centric**. Par conséquent, les attributs XML sont mappés sur les colonnes de l'ensemble de lignes. Le paramètre *rowpattern* spécifié en tant que `/ROOT/Customer` identifie les nœuds `<Customers>` à traiter.  
  
 Le paramètre (modèle de colonne) *ColPattern* facultatif n’est pas spécifié, car le nom de colonne correspond aux noms des attributs XML.  
  
 Le fournisseur de l'ensemble de lignes `OPENXML` crée un ensemble de lignes sur deux colonnes (`CustomerID` et `ContactName`), à partir desquelles l'instruction `SELECT` extrait les colonnes nécessaires (toutes les colonnes dans le cas présent).  
  
```  
DECLARE @idoc int, @doc varchar(1000);  
SET @doc ='  
<ROOT>  
<Customer CustomerID="VINET" ContactName="Paul Henriot">  
   <Order CustomerID="VINET" EmployeeID="5" OrderDate="1996-07-04T00:00:00">  
      <OrderDetail OrderID="10248" ProductID="11" Quantity="12"/>  
      <OrderDetail OrderID="10248" ProductID="42" Quantity="10"/>  
   </Order>  
</Customer>  
<Customer CustomerID="LILAS" ContactName="Carlos Gonzlez">  
   <Order CustomerID="LILAS" EmployeeID="3" OrderDate="1996-08-16T00:00:00">  
      <OrderDetail OrderID="10283" ProductID="72" Quantity="3"/>  
   </Order>  
</Customer>  
</ROOT>';  
--Create an internal representation of the XML document.  
EXEC sp_xml_preparedocument @idoc OUTPUT, @doc;  
-- Execute a SELECT statement that uses the OPENXML rowset provider.  
SELECT    *  
FROM       OPENXML (@idoc, '/ROOT/Customer',1)  
            WITH (CustomerID  varchar(10),  
                  ContactName varchar(20));  
  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
CustomerID ContactName            
---------- --------------------   
VINET      Paul Henriot  
LILAS      Carlos Gonzlez  
```  
  
 Si la même instruction `SELECT` est exécutée lorsque la valeur de *flags* est `2` (mappage **element-centric**), les valeurs de `CustomerID` et `ContactName` pour les deux clients dans le document XML sont retournées en tant que valeurs NULL, puisqu’il n’existe pas d’éléments nommés `CustomerID` ou `ContactName` dans le document XML.  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
CustomerID ContactName  
---------- -----------  
NULL       NULL  
NULL       NULL  
```  
  
### <a name="b-specifying-colpattern-for-mapping-between-columns-and-the-xml-attributes"></a>B. Spécification de ColPattern pour le mappage des colonnes aux attributs XML  
 La requête suivante retourne l'identificateur du client, la date de commande, la référence du produit et les attributs de quantité à partir du document XML. *rowpattern* identifie les éléments `<OrderDetails>`. `ProductID` et `Quantity` sont les attributs de l'élément `<OrderDetails>`. Toutefois, `OrderID`, `CustomerID` et `OrderDate` sont les attributs de l'élément parent (`<Orders>`).  
  
 Le paramètre *ColPattern* facultatif est spécifié. Cela signifie que :  
  
-   Dans l’ensemble de lignes, `OrderID`, `CustomerID` et `OrderDate` sont mappés sur les attributs du parent des nœuds identifiés par *rowpattern* dans le document XML.  
  
-   La colonne `ProdID` de l’ensemble de lignes est mappée sur l’attribut `ProductID` et la colonne `Qty` de l’ensemble de lignes sur l’attribut `Quantity` des nœuds identifiés dans *rowpattern*.  
  
 Bien que le mappage **element-centric** soit défini par le paramètre *flags*, il est supplanté par le mappage spécifié dans *ColPattern*.  
  
```  
DECLARE @idoc int, @doc varchar(1000);   
SET @doc ='  
<ROOT>  
<Customer CustomerID="VINET" ContactName="Paul Henriot">  
   <Order OrderID="10248" CustomerID="VINET" EmployeeID="5"   
           OrderDate="1996-07-04T00:00:00">  
      <OrderDetail ProductID="11" Quantity="12"/>  
      <OrderDetail ProductID="42" Quantity="10"/>  
   </Order>  
</Customer>  
<Customer CustomerID="LILAS" ContactName="Carlos Gonzlez">v  
   <Order OrderID="10283" CustomerID="LILAS" EmployeeID="3"   
           OrderDate="1996-08-16T00:00:00">  
      <OrderDetail ProductID="72" Quantity="3"/>  
   </Order>  
</Customer>  
</ROOT>';   
  
--Create an internal representation of the XML document.  
EXEC sp_xml_preparedocument @idoc OUTPUT, @doc;   
  
-- SELECT stmt using OPENXML rowset provider  
SELECT *  
FROM   OPENXML (@idoc, '/ROOT/Customer/Order/OrderDetail',2)   
         WITH (OrderID       int         '../@OrderID',   
               CustomerID  varchar(10) '../@CustomerID',   
               OrderDate   datetime    '../@OrderDate',   
               ProdID      int         '@ProductID',   
               Qty         int         '@Quantity');  
  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
OrderID CustomerID           OrderDate                 ProdID    Qty  
------------------------------------------------------------------------  
10248      VINET       1996-07-04 00:00:00.000   11      12  
10248      VINET       1996-07-04 00:00:00.000   42      10  
10283      LILAS       1996-08-16 00:00:00.000   72      3  
```  
  
### <a name="c-obtaining-results-in-an-edge-table-format"></a>C. Obtention des résultats dans un format de table edge  
 Le document XML de cet exemple comporte les éléments `<Customers>`, `<Orders>` et `<Order_0020_Details>`. La procédure stockée **sp_xml_preparedocument** est d’abord appelée pour obtenir un descripteur de document. Ce descripteur de document est transmis à `OPENXML`.  
  
 Dans l’instruction `OPENXML`, *rowpattern* (`/ROOT/Customers`) identifie les nœuds `<Customers>` à traiter. La clause WITH n’est pas fournie. Par conséquent, `OPENXML` retourne l’ensemble de lignes dans un format de table **edge**.  
  
 Enfin, l’instruction `SELECT` extrait toutes les colonnes dans la table **edge**.  
  
```  
DECLARE @idoc int, @doc varchar(1000);   
SET @doc ='  
<ROOT>  
<Customers CustomerID="VINET" ContactName="Paul Henriot">  
   <Orders CustomerID="VINET" EmployeeID="5" OrderDate=  
           "1996-07-04T00:00:00">  
      <Order_x0020_Details OrderID="10248" ProductID="11" Quantity="12"/>  
      <Order_x0020_Details OrderID="10248" ProductID="42" Quantity="10"/>  
   </Orders>  
</Customers>  
<Customers CustomerID="LILAS" ContactName="Carlos Gonzlez">  
   <Orders CustomerID="LILAS" EmployeeID="3" OrderDate=  
           "1996-08-16T00:00:00">  
      <Order_x0020_Details OrderID="10283" ProductID="72" Quantity="3"/>  
   </Orders>  
</Customers>  
</ROOT>';  
  
--Create an internal representation of the XML document.  
EXEC sp_xml_preparedocument @idoc OUTPUT, @doc;   
  
-- SELECT statement that uses the OPENXML rowset provider.  
SELECT    *  
FROM       OPENXML (@idoc, '/ROOT/Customers')   
EXEC sp_xml_removedocument @idoc;  
  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Exemples : utilisation de OPENXML](../../relational-databases/xml/examples-using-openxml.md)  
  
  
