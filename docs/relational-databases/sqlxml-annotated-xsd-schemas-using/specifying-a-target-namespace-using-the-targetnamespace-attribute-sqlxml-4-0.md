---
title: Spécification d’une cible Namespace à l’aide de l’attribut (SQLXML 4.0) targetNamespace | Documents Microsoft
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: sqlxml
ms.reviewer: ''
ms.suite: sql
ms.technology: xml
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- namespaces [SQLXML], annotated XSD schemas
- targetNamespace attribute
- XSD schemas [SQLXML], target namespaces
- annotated XSD schemas, target namespaces
- xsd:targetNamespace
- attributeFormDefault attribute
- elementFormDefault attribute
- target namespaces [SQLXML]
ms.assetid: f3df9877-6672-4444-8245-2670063c9310
caps.latest.revision: 27
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 0c755e668f5d7360d9d37f352d1cc32295b3cf5c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="specifying-a-target-namespace-using-the-targetnamespace-attribute-sqlxml-40"></a>Spécification d'un espace de noms cible à l'aide de l'attribut targetNamespace (SQLXML 4.0)
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  Dans l’écriture de schémas XSD, vous pouvez utiliser le schéma XSD **targetNamespace** attribut pour spécifier un espace de noms cible. Cette rubrique décrit comment le schéma XSD **targetNamespace**, **elementFormDefault**, et **attributeFormDefault** travail, comment ils affectent l’instance XML qui est généré et comment les requêtes XPath sont spécifiées avec les espaces de noms d’attributs.  
  
 Vous pouvez utiliser la **xsd : targetNamespace** attribut pour placer des éléments et attributs de l’espace de noms par défaut dans un autre espace de noms. Vous pouvez également spécifier si les éléments et attributs du schéma déclarés localement doivent apparaître qualifiés par un espace de noms, soit explicitement en utilisant un préfixe, soit implicitement par défaut. Vous pouvez utiliser la **elementFormDefault** et **attributeFormDefault** attributs sur le  **\<xsd : Schema >** élément pour spécifier globalement la qualification des éléments locaux et des attributs ou vous pouvez utiliser la **formulaire** attribut pour spécifier les différents éléments et attributs séparément.  
  
## <a name="examples"></a>Exemples  
 Pour créer des exemples fonctionnels à l'aide des exemples suivants, vous devez répondre à certaines conditions requises. Pour plus d’informations, consultez [configuration requise pour exécuter les exemples de SQLXML](../../relational-databases/sqlxml/requirements-for-running-sqlxml-examples.md).  
  
### <a name="a-specifying-a-target-namespace"></a>A. Spécification d'un espace de noms cible  
 Le schéma XSD suivant spécifie un espace de noms cible à l’aide de la **xsd : targetNamespace** attribut. Le schéma définit également la **elementFormDefault** et **attributeFormDefault** pour les valeurs d’attribut **« unqualified »** (la valeur par défaut pour ces attributs). Il s’agit d’une déclaration globale qui affecte tous les éléments locaux (**\<ordre >** dans le schéma) et les attributs (**CustomerID**, **ContactName**, et **OrderID** dans le schéma).  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema"  
            xmlns:CO="urn:MyNamespace"   
            targetNamespace="urn:MyNamespace" >  
<xsd:annotation>  
  <xsd:appinfo>  
    <sql:relationship name="CustOrders"  
          parent="Sales.Customer"  
          parent-key="CustomerID"  
          child="Sales.SalesOrderHeader"  
          child-key="CustomerID" />  
  </xsd:appinfo>  
</xsd:annotation>  
  
  <xsd:element name="Customer"   
               sql:relation="Sales.Customer"   
               type="CO:CustomerType" />  
  
  <xsd:complexType name="CustomerType" >  
     <xsd:sequence>  
        <xsd:element name="Order"   
                     sql:relation="Sales.SalesOrderHeader"  
                     sql:relationship="CustOrders"  
                     type="CO:OrderType" />  
     </xsd:sequence>  
        <xsd:attribute name="CustomerID"   type="xsd:string" />   
        <xsd:attribute name="SalesPersonID"  type="xsd:string" />  
  </xsd:complexType>  
  <xsd:complexType name="OrderType" >  
     <xsd:attribute name="SalesOrderID" type="xsd:integer" />  
     <xsd:attribute name="CustomerID" type="xsd:string" />  
  </xsd:complexType>  
</xsd:schema>  
```  
  
 Dans le schéma :  
  
-   Le **CustomerType** et **OrderType** des déclarations de type globales et, par conséquent, sont incluses dans l’espace de noms cible du schéma. Par conséquent, lorsque ces types sont référencés dans la déclaration de  **\<client >** élément et ses  **\<ordre >** élément enfant, un préfixe est spécifié, qui est associé à l’espace de noms cible.  
  
-   Le  **\<client >** élément est également inclus dans l’espace de noms cible du schéma, car il s’agit d’un élément global dans le schéma.  
  
 Exécutez la requête XPath suivante sur le schéma :  
  
```  
(/CO:Customer[@CustomerID=1)   
```  
  
 La requête XPath génère ce document d'instance (seules quelques commandes sont affichées) :  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
  <y0:Customer xmlns:y0="urn:MyNamespace"   
      CustomerID="ALFKI" ContactName="Maria Anders">  
        <Order CustomerID="ALFKI" OrderID="10643" />   
        <Order CustomerID="ALFKI" OrderID="10692" />   
        ...  
  </y0:Customer>  
  </ROOT>  
```  
  
 Ce document d’instance définit l’espace de noms urn : MyNamespace et associe un préfixe (y0) à ce dernier. Le préfixe est appliqué uniquement à la  **\<client >** élément global. (L’élément est global car il est déclaré en tant qu’enfant de  **\<xsd : Schema >** élément dans le schéma.)  
  
 Le préfixe n’est pas appliqué pour les éléments et attributs locaux car la valeur de **elementFormDefault** et **attributeFormDefault** attributs est défini sur **« unqualified »** dans le schéma. Notez que la  **\<ordre >** élément est local car sa déclaration apparaît en tant qu’enfant de la  **\<complexType >** élément qui définit les  **\<CustomerType >** élément. De même, les attributs (**CustomerID**, **OrderID**, et **ContactName**) sont locaux, et non globaux.  
  
##### <a name="to-create-a-working-sample-of-this-schema"></a>Pour créer un exemple fonctionnel de ce schéma  
  
1.  Copiez le code de schéma ci-dessus et collez-le dans un fichier texte. Enregistrez le fichier sous le nom targetNameSpace.xml.  
  
2.  Copiez le modèle suivant et collez-le dans un fichier texte. Enregistrez ce fichier sous le nom targetNameSpaceT.xml dans le répertoire où vous avez enregistré le fichier targetNameSpace.xml.  
  
    ```  
    <ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
      <sql:xpath-query mapping-schema="targetNamespace.xml"  
                       xmlns:CO="urn:MyNamespace" >  
        /CO:Customer[@CustomerID=1]  
      </sql:xpath-query>  
    </ROOT>  
    ```  
  
     La requête XPath dans le modèle retourne la  **\<client >** élément pour le client avec un CustomerID de 1. Notez que la requête XPath spécifie le préfixe d'espace de noms pour l'élément dans la requête et pas pour l'attribut. (Les attributs locaux ne sont pas qualifiés, comme spécifié dans le schéma.)  
  
     Le chemin d'accès au répertoire spécifié pour le schéma de mappage (targetNameSpace.xml) est relatif au répertoire dans lequel le modèle est enregistré. Vous pouvez également spécifier un chemin d'accès absolu, par exemple :  
  
    ```  
    mapping-schema="C:\MyDir\targetNamepsace.xml"  
    ```  
  
3.  Créez et utilisez le script de test SQLXML 4.0 (Sqlxml4test.vbs) pour exécuter le modèle.  
  
     Pour plus d’informations, consultez [à l’aide d’ADO pour exécuter des requêtes SQLXML](../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md).  
  
 Si le schéma spécifie **elementFormDefault** et **attributeFormDefault** attributs avec la valeur **« qualified »**, l’instance de document aura tous les éléments et attributs locaux qualifiés. Vous pouvez modifier le schéma précédent pour inclure ces attributs dans le  **\<xsd : Schema >** élément et exécuter de nouveau le modèle. Dans la mesure où les attributs sont désormais également qualifiés dans l'instance, la requête XPath sera modifiée pour inclure le préfixe d'espace de noms.  
  
 La requête XPath modifiée est présentée ci-dessous :  
  
```  
/CO:Customer[@CO:CustomerID=1]  
```  
  
 Le document XML renvoyé est présenté ci-dessous :  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
   <y0:Customer xmlns:y0="urn:MyNamespace" CustomerID="1" SalesPersonID="280">  
      <Order SalesOrderID="43860" CustomerID="1" />   
      <Order SalesOrderID="44501" CustomerID="1" />   
      <Order SalesOrderID="45283" CustomerID="1" />   
      <Order SalesOrderID="46042" CustomerID="1" />   
   </y0:Customer>  
</ROOT>  
```  
  
  
