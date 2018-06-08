---
title: Spécifier des métapropriétés dans OPENXML | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: xml
ms.reviewer: ''
ms.suite: sql
ms.technology: xml
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- overflow in XML document [SQL Server]
- metaproperties [XML in SQL Server]
- unconsumed data
- extracting information of XML nodes [SQL Server]
- OPENXML statement, metaproperties
ms.assetid: 29bfd1c6-3f9a-43c4-924a-53d438e442f4
caps.latest.revision: 23
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 83908645f3578cbb29579d11ecaf19aa8cbac0b3
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="specify-metaproperties-in-openxml"></a>Spécifier des métapropriétés dans OPENXML
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]
  Les attributs de métapropriétés dans un document XML décrivent les propriétés d'un élément XML (élément, attribut ou tout autre nœud DOM). Ces attributs n'existent pas physiquement dans le texte du document XML. Toutefois, OPENXML fournit ces métapropriétés pour tous les éléments XML. Ces métapropriétés vous permettent d'extraire des informations (par exemple des informations de positionnement local et d'espace de noms) sur les nœuds XML. Ces informations vous procurent davantage de détails que ceux apparents dans la représentation textuelle.  
  
 Vous pouvez mapper ces métapropriétés aux colonnes de l’ensemble de lignes dans une instruction OPENXML à l’aide du paramètre *ColPattern* . Les colonnes contiendront les valeurs des métapropriétés avec lesquelles elles sont mappées. Pour plus d’informations sur la syntaxe d’OPENXML, consultez [OPENXML &#40;Transact-SQL&#41;](../../t-sql/functions/openxml-transact-sql.md).  
  
 Pour accéder aux attributs de métapropriétés, un espace de noms spécifique à SQL Server est fourni. Cet espace de noms, **urn:schemas-microsoft-com:xml-metaprop** , permet à l’utilisateur d’accéder aux attributs de métapropriétés. Si le résultat d’une requête OPENXML est retourné sous un format de table du bord, celle-ci contient une colonne par attribut de métapropriété (sauf la métapropriété **xmltext** ).  
  
 Certains attributs de métapropriétés sont utilisés à des fins de traitement. Par exemple, l’attribut de métapropriété **xmltext** permet de gérer les dépassements. Cette gestion concerne les données du document non consommées ou non traitées. L'une des colonnes de l'ensemble de lignes qui est généré par OPENXML peut être identifiée comme colonne de dépassement. Pour cela, vous devez la mapper à la métapropriété **xmltext** à l’aide du paramètre *ColPattern* . La colonne reçoit ensuite les données de dépassement. Le paramètre *flags* détermine si la colonne contient tout ou uniquement les données non consommées.  
  
 Le tableau suivant répertorie les attributs de métapropriétés détenus par chaque élément XML analysé. Ces attributs de métapropriétés sont accessibles par le biais de l’espace de noms **urn:schemas-microsoft-com:xml-metaprop**. Toute valeur définie directement par l'utilisateur dans le document XML à l'aide de ces métapropriétés est ignorée.  
  
> [!NOTE]  
>  Aucune navigation XPath ne vous permet de faire référence à ces métapropriétés.  
  
|Attribut de métapropriété|Description|  
|----------------------------|-----------------|  
|**@mp:id**|Fournit l'identificateur du nœud DOM. Cet identificateur est généré par le système et couvre l'ensemble du document. Tant que le document n'est pas réanalysé, cet ID fait référence au même nœud XML.<br /><br /> Un ID XML de valeur **0** indique que l’élément est un élément racine. Son ID XML parent vaut NULL.|  
|**@mp:localname**|Stocke la partie locale du nom du nœud. Utilisé avec un préfixe et un URI d'espace de noms, il permet de nommer les nœuds d'éléments ou d'attributs.|  
|**@mp:namespaceuri**|Fournit l'URI de l'espace de noms de l'élément actuel. Si la valeur de cet attribut est NULL, aucun espace de noms n'est présent.|  
|**@mp:prefix**|Stocke le préfixe d'espace de noms de l'élément actuel.<br /><br /> Si aucun préfixe n'est présent (NULL) et qu'un URI est fourni, l'espace de noms spécifié est l'espace de noms par défaut. Si aucun URI n'est fourni, aucun espace de noms n'est attaché.|  
|**@mp:prev**|Stocke le frère précédent relatif à un nœud. Fournit des informations sur l'ordre des éléments dans le document.<br /><br /> **@mp:prev** contient l’ID XML du frère précédent possédant le même élément parent. Si un élément figure au début de la liste des frères, **@mp:prev** a pour valeur NULL.|  
|**@mp:xmltext**|Utilisé à des fins de traitement. Représente la sérialisation textuelle de l'élément, ainsi que de ses attributs et sous-éléments, utilisée dans la gestion de dépassement d'OPENXML.|  
  
 Ce tableau présente les propriétés parentes supplémentaires qui vous permettent d'extraire des informations sur la hiérarchie.  
  
|Attribut de métapropriété parent|Description|  
|-----------------------------------|-----------------|  
|**@mp:parentid**|Correspond à **../@mp:id**|  
|**@mp:parentlocalname**|Correspond à **../@mp:localname**|  
|**@mp:parentnamespacerui**|Correspond à **../@mp:namespaceuri**|  
|**@mp:parentprefix**|Correspond à **../@mp:prefix**|  
  
## <a name="examples"></a>Exemples  
 Les exemples suivants illustrent l'utilisation d'OPENXML pour créer différentes vues d'ensembles de lignes.  
  
### <a name="a-mapping-the-openxml-rowset-columns-to-the-metaproperties"></a>A. Mappage des colonnes de l'ensemble de lignes OPENXML aux métapropriétés  
 Cet exemple utilise OPENXML pour créer une vue d'ensemble de lignes de l'exemple de document XML. Plus spécifiquement, il montre comment différents attributs de métapropriétés peuvent être mappés à des colonnes d’ensembles de lignes dans une instruction OPENXML à l’aide du paramètre *ColPattern* .  
  
 L'instruction OPENXML contient les éléments suivants :  
  
-   Le paramètre **id** est mappée à l’attribut de métapropriété **@mp:id** , ce qui indique que la colonne contient l’ID XML système unique de l’élément.  
  
-   Le paramètre **parent** est mappée à **@mp:parentid** , ce qui indique qu’elle contient l’ID XML du parent de l’élément.  
  
-   Le paramètre **parentLocalName** est mappée à **@mp:parentlocalname** , ce qui indique que la colonne contient le nom local du parent.  
  
 L'instruction SELECT retourne ensuite l'ensemble de lignes fourni par OPENXML :  
  
```  
DECLARE @idoc int  
DECLARE @doc nvarchar(1000)  
-- Sample XML document  
SET @doc = N'<root>  
  <Customer cid= "C1" name="Janine" city="Issaquah">  
      <Order oid="O1" date="1/20/1996" amount="3.5" />  
      <Order oid="O2" date="4/30/1997" amount="13.4">Customer was very satisfied</Order>  
   </Customer>  
   <Customer cid="C2" name="Ursula" city="Oelde" >  
      <Order oid="O3" date="7/14/1999" amount="100" note="Wrap it blue white red">  
          <Urgency>Important</Urgency>  
      </Order>  
      <Order oid="O4" date="1/20/1996" amount="10000"/>  
   </Customer>  
</root>'  
-- Create an internal representation of the XML document.  
EXEC sp_xml_preparedocument @idoc OUTPUT, @doc  
  
-- Execute a SELECT statement using OPENXML rowset provider.  
SELECT *  
FROM OPENXML (@idoc, '/root/Customer/Order', 9)  
      WITH (id int '@mp:id',   
            oid char(5),   
            date datetime,   
            amount real,   
            parentIDNo int '@mp:parentid',   
            parentLocalName varchar(40) '@mp:parentlocalname')  
EXEC sp_xml_removedocument @idoc  
```  
  
 Voici le résultat obtenu :  
  
```  
id   oid         date                amount    parentIDNo  parentLocalName    
--- ------- ---------------------- ---------- ------------ ---------------  
6    O1    1996-01-20 00:00:00.000     3.5         2        Customer  
10   O2    1997-04-30 00:00:00.000     13.4        2        Customer  
19   O3    1999-07-14 00:00:00.000     100.0       15       Customer  
25   O4    1996-01-20 00:00:00.000     10000.0     15       Customer  
```  
  
### <a name="b-retrieving-the-whole-xml-document"></a>B. Récupération du document XML en entier  
 Dans cet exemple, OPENXML est utilisé pour créer une vue d'ensemble de lignes contenant une seule colonne à partir de l'exemple de document XML. Cette colonne, **Col1**, est mappée à la métapropriété **xmltext** et devient une colonne de dépassement. En conséquence, elle reçoit les données non consommées. Dans ce cas, il s'agit du document en entier.  
  
 L'instruction SELECT retourne ensuite l'intégralité de l'ensemble de lignes.  
  
```  
DECLARE @idoc int  
DECLARE @doc nvarchar(1000)  
SET @doc = N'<?xml version="1.0"?>  
<root>  
  <Customer cid= "C1" name="Janine" city="Issaquah">  
      <Order oid="O1" date="1/20/1996" amount="3.5" />  
      <Order oid="O2" date="4/30/1997" amount="13.4">Customer was very   
             satisfied</Order>  
   </Customer>  
   <Customer cid="C2" name="Ursula" city="Oelde" >  
      <Order oid="O3" date="7/14/1999" amount="100" note="Wrap it blue   
             white red">  
     <MyTag>Testing to see if all the subelements are returned</MyTag>  
          <Urgency>Important</Urgency>  
      </Order>  
      <Order oid="O4" date="1/20/1996" amount="10000"/>  
   </Customer>  
</root>'  
-- Create an internal representation of the XML document.  
EXEC sp_xml_preparedocument @idoc OUTPUT, @doc  
  
-- Execute a SELECT statement using OPENXML rowset provider.  
SELECT *  
FROM OPENXML (@idoc, '/')  
   WITH (Col1 ntext '@mp:xmltext')  
```  
  
 Pour extraire l'intégralité du document sans déclaration XML, vous pouvez spécifier la requête comme suit :  
  
```  
SELECT *  
FROM OPENXML (@idoc, '/root')  
   WITH (Col1 ntext '@mp:xmltext')  
EXEC sp_xml_removedocument @idoc  
```  
  
 La requête retourne l'élément racine ayant la racine du nom et les données contenues dans l'élément racine.  
  
### <a name="c-specifying-the-xmltext-metaproperty-to-retrieve-the-unconsumed-data-in-a-column"></a>C. Spécification de la métapropriété xmltext pour récupérer les données non consommées d'une colonne  
 Cet exemple utilise OPENXML pour créer une vue d'ensemble de lignes de l'exemple de document XML. Il montre comment récupérer des données XML non consommées en mappant l’attribut de métapropriété **xmltext** à une colonne d’ensemble de lignes dans OPENXML.  
  
 Le paramètre **comment** est mappée à la métapropriété **@mp:xmltext** ). Le paramètre *flags* a la valeur **9** (XML_ATTRIBUTE et XML_NOCOPY). Cela indique un mappage **centré sur l’attribut** et signifie que seules les données non consommées doivent être copiées dans la colonne de dépassement.  
  
 L'instruction SELECT retourne ensuite l'ensemble de lignes fourni par OPENXML.  
  
 Dans cet exemple, la métapropriété **@mp:parentlocalname** est définie pour une colonne ( **ParentLocalName**) de l’ensemble de lignes généré par OPENXML. Par conséquent, cette colonne contient le nom local de l'élément parent.  
  
 Deux autres colonnes sont spécifiées dans l’ensemble de lignes ( **parent** et **comment**). Le paramètre **parent** est mappée à **@mp:parentid** , ce qui indique qu’elle contient l’ID XML de l’élément parent de l’élément. La colonne comment est mappée à la métapropriété **@mp:xmltext** ).  
  
```  
DECLARE @idoc int  
DECLARE @doc nvarchar(1000)  
-- sample XML document  
SET @doc = N'<root>  
  <Customer cid= "C1" name="Janine" city="Issaquah">  
      <Order oid="O1" date="1/20/1996" amount="3.5" />  
      <Order oid="O2" date="4/30/1997" amount="13.4">Customer was very satisfied</Order>  
   </Customer>  
   <Customer cid="C2" name="Ursula" city="Oelde" >  
      <Order oid="O3" date="7/14/1999" amount="100" note="Wrap it blue white red">  
          <Urgency>Important</Urgency>  
      </Order>  
      <Order oid="O4" date="1/20/1996" amount="10000"/>  
   </Customer>  
</root>  
'  
-- Create an internal representation of the XML document.  
EXEC sp_xml_preparedocument @idoc OUTPUT, @doc  
  
-- Execute a SELECT statement using OPENXML rowset provider.  
SELECT *  
FROM OPENXML (@idoc, '/root/Customer/Order', 9)  
      WITH (oid char(5),   
            date datetime,  
            comment ntext '@mp:xmltext')  
EXEC sp_xml_removedocument @idoc  
```  
  
 Voici l'ensemble de résultats. Les données oid et date étant déjà consommées, elles n'apparaissent pas dans la colonne de dépassement.  
  
```  
oid   date                        comment                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            
----- --------------------------- ----------------------------------------  
O1    1996-01-20 00:00:00.000     <Order amount="3.5"/>  
O2    1997-04-30 00:00:00.000     <Order amount="13.4">Customer was very   
                                   satisfied</Order>  
O3    1999-07-14 00:00:00.000     <Order amount="100" note="Wrap it blue   
                                   white red"><Urgency>   
                                   Important</Urgency></Order>  
O4    1996-01-20 00:00:00.000     <Order amount="10000"/>  
```  
  
## <a name="see-also"></a> Voir aussi  
 [OPENXML &#40;Transact-SQL&#41;](../../t-sql/functions/openxml-transact-sql.md)   
 [OPENXML &#40;SQL Server&#41;](../../relational-databases/xml/openxml-sql-server.md)  
  
  
