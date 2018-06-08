---
title: 'Spécification de la profondeur dans les relations récursives à l’aide de SQL : max-depth | Documents Microsoft'
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: sqlxml
ms.reviewer: ''
ms.suite: sql
ms.technology: xml
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- max-depth annotation
- XPath queries [SQLXML], recursive relationships
- depth in recursive relationships [SQLXML]
- annotated XSD schemas, recursive relationships
- relationships [SQLXML], recursive relationships
- self joins
- recursive relationships [SQLXML]
- recursion [SQLXML]
- sql:max-depth
- recursive joins [SQLXML]
ms.assetid: 0ffdd57d-dc30-44d9-a8a0-f21cadedb327
caps.latest.revision: 26
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 025ad34e3aca3ea4330c9a5878f834c605bf645c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="specifying-depth-in-recursive-relationships-by-using-sqlmax-depth"></a>Spécification de la profondeur dans les relations récursives à l'aide de sql:max-depth
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  Dans les bases de données relationnelles, lorsqu'une table est impliquée dans une relation avec elle-même, on utilise le terme de relation récursive. Par exemple, dans une relation responsable-subalterne, une table qui stocke des enregistrements d'employés est impliquée dans une relation avec elle-même. Dans ce cas, la table d'employés joue un rôle de responsable d'un côté de la relation, et la même table joue un rôle de subalterne de l'autre côté.  
  
 Les schémas de mappage peuvent inclure des relations récursives dans lesquelles un élément et son ancêtre sont du même type.  
  
## <a name="example-a"></a>Exemple A  
 Considérez la table ci-dessous :  
  
```  
Emp (EmployeeID, FirstName, LastName, ReportsTo)  
```  
  
 Dans cette table, la colonne ReportsTo stocke l'ID employé du responsable.  
  
 Supposez que vous souhaitez générer une hiérarchie XML des employés, dans laquelle l'employé responsable est en haut de la hiérarchie et les employés subalternes apparaissent dans la hiérarchie correspondante comme illustré dans le fragment d'exemple XML suivant. Ce fragment montre est la *arborescence récursive* pour l’employé 1.  
  
```  
<?xml version="1.0" encoding="utf-8" ?>   
<root>  
  <Emp FirstName="Nancy" EmployeeID="1" LastName="Devolio">  
     <Emp FirstName="Andrew" EmployeeID="2" LastName="Fuller" />   
     <Emp FirstName="Janet" EmployeeID="3" LastName="Leverling">  
        <Emp FirstName="Margaret" EmployeeID="4" LastName="Peacock">  
          <Emp FirstName="Steven" EmployeeID="5" LastName="Devolio">  
...  
...  
</root>  
```  
  
 Dans ce fragment, l'employé 5 est le subalterne de l'employé 4, l'employé 4 est le subalterne de l'employé 3 et les employés 3 et 2 sont les subalternes de l'employé 1.  
  
 Pour produire ce résultat, vous pouvez utiliser le schéma XSD suivant et spécifier une requête XPath contre lui. Le schéma décrit un  **\<Emp >** élément de type EmployeeType, constitué d’un  **\<Emp >** élément enfant du même type, EmployeeType. Il s'agit d'une relation récursive (l'élément et son ancêtre sont du même type). En outre, le schéma utilise un  **\<SQL : Relationship >** pour décrire la relation parent-enfant entre le responsable et le subalterne. Notez que dans ce  **\<SQL : Relationship >**, Emp est le parent et la table enfant.  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
            xmlns:dt="urn:schemas-microsoft-com:datatypes"  
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
  <xsd:annotation>  
    <xsd:appinfo>  
      <sql:relationship name="SupervisorSupervisee"  
                                  parent="Emp"  
                                  parent-key="EmployeeID"  
                                  child="Emp"  
                                  child-key="ReportsTo" />  
    </xsd:appinfo>  
  </xsd:annotation>  
  <xsd:element name="Emp" type="EmployeeType"   
                          sql:relation="Emp"   
                          sql:key-fields="EmployeeID"   
                          sql:limit-field="ReportsTo" />  
  <xsd:complexType name="EmployeeType">  
    <xsd:sequence>  
      <xsd:element name="Emp" type="EmployeeType"   
                              sql:relation="Emp"   
                              sql:key-fields="EmployeeID"  
                              sql:relationship="SupervisorSupervisee"  
                              sql:max-depth="6" />  
    </xsd:sequence>   
    <xsd:attribute name="EmployeeID" type="xsd:ID" />  
    <xsd:attribute name="FirstName" type="xsd:string"/>  
    <xsd:attribute name="LastName" type="xsd:string"/>  
  </xsd:complexType>  
</xsd:schema>  
```  
  
 La relation étant récursive, vous devez trouver un moyen de spécifier la profondeur de récursivité dans le schéma. Sinon, le résultat sera une récursivité sans fin (employé subalterne à employé subalterne à employé, et ainsi de suite). Le **SQL : max-depth** annotation permet de spécifier la profondeur de récursivité. Dans cet exemple, pour spécifier une valeur pour **SQL : max-depth**, vous devez connaître la profondeur la hiérarchie de direction de l’entreprise.  
  
> [!NOTE]  
>  Le schéma spécifie la **SQL : limit-champ** annotation, mais ne spécifie ne pas le **SQL : limit-valeur** annotation. Cela limite le nœud supérieur dans la hiérarchie résultante uniquement aux employés qui ne sont subalternes de personne. (ReportsTo a la valeur NULL.) Spécification **SQL : limit-champ** sans spécifier de **SQL : limit-valeur** (qui utilise par défaut NULL) annotation accomplit cela. Si vous souhaitez que le XML résultant inclue chaque direction possible arborescence (l’arborescence de création de rapports pour chaque employé dans la table), supprimez le **SQL : limit-champ** annotation à partir du schéma.  
  
> [!NOTE]  
>  La procédure suivante utilise la base de données tempdb.  
  
#### <a name="to-test-a-sample-xpath-query-against-the-schema"></a>Pour tester un exemple de requête XPath sur le schéma  
  
1.  Créez un exemple de table nommé Emp dans la base de données tempdb vers laquelle la racine virtuelle pointe.  
  
    ```  
    USE tempdb  
    CREATE TABLE Emp (  
           EmployeeID int primary key,   
           FirstName  varchar(20),   
           LastName   varchar(20),   
           ReportsTo int)  
    ```  
  
2.  Ajoutez les exemples de données suivants :  
  
    ```  
    INSERT INTO Emp values (1, 'Nancy', 'Devolio',NULL)  
    INSERT INTO Emp values (2, 'Andrew', 'Fuller',1)  
    INSERT INTO Emp values (3, 'Janet', 'Leverling',1)  
    INSERT INTO Emp values (4, 'Margaret', 'Peacock',3)  
    INSERT INTO Emp values (5, 'Steven', 'Devolio',4)  
    INSERT INTO Emp values (6, 'Nancy', 'Buchanan',5)  
    INSERT INTO Emp values (7, 'Michael', 'Suyama',6)  
    ```  
  
3.  Copiez le code de schéma ci-dessus et collez-le dans un fichier texte. Enregistrez le fichier en tant que maxDepth.xml.  
  
4.  Copiez le modèle suivant et collez-le dans un fichier texte. Enregistrez le fichier en tant que maxDepthT.xml dans le même répertoire où vous avez enregistré maxDepth.xml. La requête dans le modèle retourne tous les employés de la table Emp.  
  
    ```  
    <ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
      <sql:xpath-query mapping-schema="maxDepth.xml">  
        /Emp  
      </sql:xpath-query>  
    </ROOT>  
    ```  
  
     Le chemin d'accès au répertoire spécifié pour le schéma de mappage (maxDepth.xml) est relatif au répertoire où le modèle est enregistré. Vous pouvez également spécifier un chemin d'accès absolu, par exemple :  
  
    ```  
    mapping-schema="C:\MyDir\maxDepth.xml"  
    ```  
  
5.  Créez et utilisez le script de test SQLXML 4.0 (Sqlxml4test.vbs) pour exécuter le modèle. Pour plus d’informations, consultez [à l’aide d’ADO pour exécuter des requêtes SQLXML 4.0](../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md).  
  
 Voici le résultat obtenu :  
  
```  
<?xml version="1.0" encoding="utf-8" ?>   
<root>  
  <Emp FirstName="Nancy" EmployeeID="1" LastName="Devolio">  
  <Emp FirstName="Andrew" EmployeeID="2" LastName="Fuller" />   
    <Emp FirstName="Janet" EmployeeID="3" LastName="Leverling">  
      <Emp FirstName="Margaret" EmployeeID="4" LastName="Peacock">  
        <Emp FirstName="Steven" EmployeeID="5" LastName="Devolio">  
          <Emp FirstName="Nancy" EmployeeID="6" LastName="Buchanan">  
            <Emp FirstName="Michael" EmployeeID="7" LastName="Suyama" />   
          </Emp>  
        </Emp>  
      </Emp>  
    </Emp>  
  </Emp>  
</root>  
```  
  
> [!NOTE]  
>  Pour produire différentes profondeurs de hiérarchies dans le résultat, modifiez la valeur de la **SQL : max-depth** annotation dans le schéma et exécuter le modèle après chaque modification.  
  
 Dans le schéma précédent, tous les  **\<Emp >** éléments avaient exactement le même jeu d’attributs (**EmployeeID**, **FirstName**, et **LastName**). Le schéma suivant a été légèrement modifié pour retourner un autre **ReportsTo** attribut pour tous les le  **\<Emp >** les éléments d’un responsable.  
  
 Par exemple, ce fragment XML affiche les subalternes de l'employé 1 :  
  
```  
<?xml version="1.0" encoding="utf-8" ?>   
<root>  
<Emp FirstName="Nancy" EmployeeID="1" LastName="Devolio">  
  <Emp FirstName="Andrew" EmployeeID="2"   
       ReportsTo="1" LastName="Fuller" />   
  <Emp FirstName="Janet" EmployeeID="3"   
       ReportsTo="1" LastName="Leverling">  
...  
...  
```  
  
 Voici le schéma modifié :  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
            xmlns:dt="urn:schemas-microsoft-com:datatypes"  
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
  <xsd:annotation>  
    <xsd:documentation>  
      Customer-Order-Order Details Schema  
      Copyright 2000 Microsoft. All rights reserved.  
    </xsd:documentation>  
    <xsd:appinfo>  
      <sql:relationship name="SupervisorSupervisee"   
                  parent="Emp"  
                  parent-key="EmployeeID"  
                  child="Emp"  
                  child-key="ReportsTo" />  
    </xsd:appinfo>  
  </xsd:annotation>  
  <xsd:element name="Emp"   
                   type="EmpType"   
                   sql:relation="Emp"   
                   sql:key-fields="EmployeeID"   
                   sql:limit-field="ReportsTo" />  
  <xsd:complexType name="EmpType">  
    <xsd:sequence>  
       <xsd:element name="Emp"   
                    type="EmpType"   
                    sql:relation="Emp"   
                    sql:key-fields="EmployeeID"  
                    sql:relationship="SupervisorSupervisee"  
                    sql:max-depth="6"/>  
    </xsd:sequence>   
    <xsd:attribute name="EmployeeID" type="xsd:int" />  
    <xsd:attribute name="FirstName" type="xsd:string"/>  
    <xsd:attribute name="LastName" type="xsd:string"/>  
    <xsd:attribute name="ReportsTo" type="xsd:int" />  
  </xsd:complexType>  
</xsd:schema>  
```  
  
## <a name="sqlmax-depth-annotation"></a>Annotation sql:max-depth  
 Dans un schéma constitué de relations récursives, la profondeur de récursivité doit être spécifiée explicitement dans le schéma. Cela est nécessaire afin de produire avec succès la requête FOR XML EXPLICIT correspondante qui retourne les résultats demandés.  
  
 Utilisez le **SQL : max-depth** annotation dans le schéma pour spécifier la profondeur de récursivité dans une relation récursive décrite dans le schéma. La valeur de la **SQL : max-profondeur** annotation est un entier positif (1 à 50) qui indique le nombre de récurrences : une valeur de 1 arrête la récursivité à l’élément pour lequel la **SQL : max-profondeur** annotation est spécifiée, une valeur de 2 arrête la récursivité au niveau suivant de l’élément auquel **SQL : max-profondeur** est spécifié ; et ainsi de suite.  
  
> [!NOTE]  
>  Dans l'implémentation sous-jacente, une requête XPath spécifiée contre un schéma de mappage est convertie en requête SELECT ... FOR XML EXPLICIT. Cette requête nécessite que vous spécifiiez une profondeur de récursivité finie. Plus la valeur que vous spécifiez pour **SQL : max-depth**, plus la requête FOR XML EXPLICIT qui est généré. Cela risque d'allonger la durée de récupération.  
  
> [!NOTE]  
>  Les codes de mise à jour (updategrams) et chargements en masse XML ignorent l'annotation max-depth. Cela signifie que les insertions ou mises à jour récursives ont lieu indépendamment de la valeur que vous spécifiez pour max-depth.  
  
## <a name="specifying-sqlmax-depth-on-complex-elements"></a>Spécification de sql:max-depth sur des éléments complexes  
 Le **SQL : max-depth** annotation peut être définie sur n’importe quel élément de contenu complexe.  
  
### <a name="recursive-elements"></a>Éléments récursifs  
 Si **SQL : max-depth** est spécifié dans l’élément parent et l’élément enfant dans une relation récursive, le **SQL : max-profondeur** annotation spécifiée sur le parent est prioritaire. Par exemple, dans le schéma suivant, le **SQL : max-depth** annotation est spécifiée sur le parent et les éléments d’employé enfants. Dans ce cas, **SQL : max-depth = 4**, spécifié sur la  **\<Emp >** élément parent (pour jouer un rôle de responsable), est prioritaire. Le **SQL : max-depth** spécifié sur l’enfant  **\<Emp >** élément (jouant un rôle de subalterne) est ignoré.  
  
#### <a name="example-b"></a>Exemple B  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
            xmlns:dt="urn:schemas-microsoft-com:datatypes"  
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
  <xsd:annotation>  
    <xsd:appinfo>  
      <sql:relationship name="SupervisorSupervisee"  
                                  parent="Emp"  
                                  parent-key="EmployeeID"  
                                  child="Emp"  
                                  child-key="ReportsTo" />  
    </xsd:appinfo>  
  </xsd:annotation>  
  <xsd:element name="Emp" type="EmployeeType"   
                          sql:relation="Emp"   
                          sql:key-fields="EmployeeID"   
                          sql:limit-field="ReportsTo"   
                          sql:max-depth="3" />  
  <xsd:complexType name="EmployeeType">  
    <xsd:sequence>  
      <xsd:element name="Emp" type="EmployeeType"   
                              sql:relation="Emp"   
                              sql:key-fields="EmployeeID"  
                              sql:relationship="SupervisorSupervisee"  
                              sql:max-depth="2" />  
    </xsd:sequence>   
    <xsd:attribute name="EmployeeID" type="xsd:ID" />  
    <xsd:attribute name="FirstName" type="xsd:string"/>  
    <xsd:attribute name="LastName" type="xsd:string"/>  
  </xsd:complexType>  
</xsd:schema>  
```  
  
 Pour tester ce schéma, suivez les étapes fournies pour un exemple, plus haut dans cette rubrique.  
  
### <a name="nonrecursive-elements"></a>Éléments non récursifs  
 Si le **SQL : max-depth** annotation est spécifiée sur un élément dans le schéma qui ne provoque pas de récursivité, il est ignoré. Dans le schéma suivant, un  **\<Emp >** élément se compose d’un  **\<constante >** élément enfant qui, à son tour, a une  **\<Emp >** élément enfant.  
  
 Dans ce schéma, le **SQL : max-profondeur** annotation spécifiée sur le  **\<constante >** élément est ignoré, car il n’existe aucune récursivité entre le  **\<Emp >** parent et le  **\<constante >** élément enfant. Mais il existe une récursivité entre le  **\<Emp >** ancêtre et  **\<Emp >** enfant. Le schéma spécifie la **SQL : max-depth** annotation sur les deux. Par conséquent, le **SQL : max-depth** annotation est spécifiée sur l’ancêtre (**\<Emp >** dans le rôle de responsable) est prioritaire.  
  
#### <a name="example-c"></a>Exemple C  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"   
xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
  <xsd:annotation>  
    <xsd:appinfo>  
      <sql:relationship name="SupervisorSupervisee"   
                  parent="Emp"   
                  child="Emp"   
                  parent-key="EmployeeID"   
                  child-key="ReportsTo"/>  
    </xsd:appinfo>  
  </xsd:annotation>  
  <xsd:element name="Emp"   
               sql:relation="Emp"   
               type="EmpType"  
               sql:limit-field="ReportsTo"  
               sql:max-depth="1" />  
    <xsd:complexType name="EmpType" >  
      <xsd:sequence>  
       <xsd:element name="Constant"   
                    sql:is-constant="1"   
                    sql:max-depth="20" >  
         <xsd:complexType >  
           <xsd:sequence>  
            <xsd:element name="Emp"   
                         sql:relation="Emp" type="EmpType"  
                         sql:relationship="SupervisorSupervisee"   
                         sql:max-depth="3" />  
         </xsd:sequence>  
         </xsd:complexType>  
         </xsd:element>  
      </xsd:sequence>  
      <xsd:attribute name="EmployeeID" type="xsd:int" />  
    </xsd:complexType>  
</xsd:schema>  
```  
  
 Pour tester ce schéma, suivez les étapes fournies pour l'Exemple A, plus haut dans cette rubrique.  
  
## <a name="complex-types-derived-by-restriction"></a>Types complexes dérivés par restriction  
 Si vous avez une dérivation de type complexe par  **\<restriction >**, éléments de type complexe de base correspondant ne peut pas spécifier le **SQL : max-depth** annotation. Dans ce cas, le **SQL : max-depth** annotation peut être ajoutée à l’élément du type dérivé.  
  
 En revanche, si vous avez une dérivation de type complexe par  **\<extension >**, les éléments de type complexe de base correspondant peuvent spécifier le **SQL : max-depth** annotation.  
  
 Par exemple, le schéma XSD suivant génère une erreur car le **SQL : max-depth** annotation est spécifiée sur le type de base. Cette annotation n’est pas pris en charge sur un type qui est dérivé en  **\<restriction >** à partir d’un autre type. Pour résoudre ce problème, vous devez modifier le schéma et spécifier le **SQL : max-depth** annotation sur l’élément dans le type dérivé.  
  
#### <a name="example-d"></a>Exemple D  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
            xmlns:dt="urn:schemas-microsoft-com:datatypes"  
            xmlns:msdata="urn:schemas-microsoft-com:mapping-schema">  
  <xsd:complexType name="CustomerBaseType">   
    <xsd:sequence>  
       <xsd:element name="CID" msdata:field="CustomerID" />  
       <xsd:element name="CompanyName"/>  
       <xsd:element name="Customers" msdata:max-depth="3">  
         <xsd:annotation>  
           <xsd:appinfo>  
             <msdata:relationship  
                     parent="Customers"  
                     parent-key="CustomerID"  
                     child-key="CustomerID"  
                     child="Customers" />  
           </xsd:appinfo>  
         </xsd:annotation>  
       </xsd:element>  
    </xsd:sequence>  
  </xsd:complexType>  
  <xsd:element name="Customers" type="CustomerType"/>  
  <xsd:complexType name="CustomerType">  
    <xsd:complexContent>  
       <xsd:restriction base="CustomerBaseType">  
          <xsd:sequence>  
            <xsd:element name="CID"   
                         type="xsd:string"/>  
            <xsd:element name="CompanyName"   
                         type="xsd:string"  
                         msdata:field="CName" />  
            <xsd:element name="Customers"   
                         type="CustomerType" />  
          </xsd:sequence>  
       </xsd:restriction>  
    </xsd:complexContent>  
  </xsd:complexType>  
</xsd:schema>   
```  
  
 Dans le schéma, **SQL : max-depth** est spécifié sur un **CustomerBaseType** type complexe. Le schéma spécifie également un  **\<client >** élément de type **CustomerType**, qui est dérivée de **CustomerBaseType**. Une requête XPath spécifiée sur un tel schéma génère une erreur, car **SQL : max-depth** n’est pas pris en charge sur un élément qui est défini dans un type de base de restriction.  
  
## <a name="schemas-with-a-deep-hierarchy"></a>Schémas avec une hiérarchie profonde  
 Vous pouvez avoir un schéma qui inclut une hiérarchie profonde dans laquelle un élément contient un élément enfant, qui à son tour contient un autre élément enfant, et ainsi de suite. Si le **SQL : max-depth** annotation spécifiée dans un tel schéma génère un document XML qui inclut une hiérarchie de plus de 500 niveaux (avec l’élément de niveau supérieur au niveau 1, son enfant au niveau 2 et ainsi de suite), une erreur est retournée.  
  
  
