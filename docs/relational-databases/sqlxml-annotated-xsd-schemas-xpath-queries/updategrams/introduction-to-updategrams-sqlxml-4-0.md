---
title: Introduction aux codes (SQLXML 4.0) | Documents Microsoft
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
- explicit schema mapping [SQLXML]
- updategrams [SQLXML], mapping schema specifying
- namespaces [SQLXML]
- updategrams [SQLXML], about updategrams
- attribute-centric mapping
- invalid characters [SQLXML]
- element-centric mapping [SQLXML]
- mapping schema [SQLXML], updategrams
- namespaces [SQLXML], updategrams
- executing updategrams [SQLXML]
- implicit schema mapping
ms.assetid: cfe24e82-a645-4f93-ab16-39c21f90cce6
caps.latest.revision: 12
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 6c3d51e64669f2410ca6e99926734b767dba1f45
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="introduction-to-updategrams-sqlxml-40"></a>Présentation des codes de mise à jour (SQLXML 4.0)
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  Vous pouvez modifier (insérer, mettre à jour ou supprimer) une base de données [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] à partir d’un XML existant de document à l’aide d’une mise à jour ou OPENXML [!INCLUDE[tsql](../../../includes/tsql-md.md)] (fonction).  
  
 La fonction OPENXML modifie une base de données en fragmentant le document XML existant et en fournissant un ensemble de lignes qui peut être passé à une instruction INSERT, UPDATE ou DELETE. Avec OPENXML, les opérations sont effectuées directement sur les tables de base de données. Par conséquent, OPENXML est particulièrement approprié partout où les fournisseurs d'ensembles de lignes, tels qu'une table, peuvent apparaître comme source.  
  
 Comme OPENXML, un code de mise à jour vous permet d'insérer, de mettre à jour ou de supprimer des données dans la base de données ; toutefois, un code de mise à jour fonctionne sur les vues XML fournies par le schéma XSD annoté (ou un XDR) ; par exemple, les mises à jour sont appliquées à la vue XML fournie par le schéma de mappage. Le schéma de mappage, à son tour, possède les informations nécessaires pour mapper des éléments et des attributs XML aux tables et colonnes de base de données correspondantes. Le code de mise à jour utilise ces informations de mappage pour mettre à jour les tables et colonnes de base de données.  
  
> [!NOTE]  
>  Cette documentation suppose une connaissance suffisante des modèles et de la prise en charge des schémas de mappage dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Pour plus d’informations, consultez [Introduction aux schémas de XSD annoté & #40 ; SQLXML 4.0 & #41 ; ](../../../relational-databases/sqlxml/annotated-xsd-schemas/introduction-to-annotated-xsd-schemas-sqlxml-4-0.md). Pour les applications héritées qui utilisent XDR, consultez [de schémas XDR annotés &#40;déconseillé dans SQLXML 4.0&#41;](../../../relational-databases/sqlxml/annotated-xsd-schemas/annotated-xdr-schemas-deprecated-in-sqlxml-4-0.md).  
  
## <a name="required-namespaces-in-the-updategram"></a>Espace de noms requis dans le code de mise à jour  
 Les mots clés dans une mise à jour, telles que  **\<synchronisation >**,  **\<avant >**, et  **\<après >**, existe dans le **urn : schemas-microsoft-com-mise à jour** espace de noms. Le préfixe d'espace de noms employé est arbitraire. Dans cette documentation, le **updg** préfixe indique le **mise à jour** espace de noms.  
  
## <a name="reviewing-syntax"></a>Vérification de la syntaxe  
 Une mise à jour est un modèle avec  **\<synchronisation >**,  **\<avant >**, et  **\<après >** blocs qui forment la syntaxe de la mise à jour. Le code ci-dessous illustre cette syntaxe dans sa forme la plus simple :  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
  <updg:sync [mapping-schema= "AnnotatedSchemaFile.xml"] >  
    <updg:before>  
        ...  
    </updg:before>  
    <updg:after>  
        ...  
    </updg:after>  
  </updg:sync>  
</ROOT>  
```  
  
 Les définitions ci-dessous décrivent le rôle de chacun de ces blocs :  
  
 **\<before>**  
 Identifie l'état existant (également appelé « état before ») de l'instance de l'enregistrement.  
  
 **\<after>**  
 Identifie le nouvel état qu'auront les données après modification.  
  
 **\<sync>**  
 Contient le  **\<avant >** et  **\<après >** blocs. A  **\<synchronisation >** bloc peut contenir plusieurs jeux de  **\<avant >** et  **\<après >** blocs. S’il existe plusieurs jeux de  **\<avant >** et  **\<après >** blocs, ces blocs (même si elles sont vides) doit être spécifié en tant que paires. En outre, une mise à jour peut avoir plusieurs  **\<synchronisation >** bloc. Chaque  **\<synchronisation >** bloc est une unité de transaction (ce qui signifie que, soit tout dans le  **\<synchronisation >** bloc est effectué ou rien ne se produit). Si vous spécifiez plusieurs  **\<synchronisation >** blocs dans une mise à jour, l’échec d’un  **\<synchronisation >** bloc n’affecte pas l’autre  **\<synchronisation >** blocs.  
  
 Indique si une mise à jour supprime, insère ou met à jour une instance d’enregistrement varie selon le contenu de la  **\<avant >** et  **\<après >** blocs :  
  
-   Si une instance d’enregistrement apparaît uniquement dans le  **\<avant >** bloc sans instance correspondante dans le  **\<après >** bloc, la mise à jour effectue une opération de suppression.  
  
-   Si une instance d’enregistrement apparaît uniquement dans le  **\<après >** bloc sans instance correspondante dans le  **\<avant >** bloc, il s’agit d’une opération d’insertion.  
  
-   Si une instance de l’enregistrement s’affiche dans le  **\<avant >** bloquer et qui dispose d’une instance correspondante dans le  **\<après >** bloc, il s’agit d’une opération de mise à jour. Dans ce cas, la mise à jour met à jour l’instance de l’enregistrement avec les valeurs qui sont spécifiés dans le  **\<après >** bloc.  
  
## <a name="specifying-a-mapping-schema-in-the-updategram"></a>Spécification d'un schéma de mappage dans le code de mise à jour  
 Dans un code de mise à jour, l'abstraction XML fournie par un schéma de mappage (les schémas XSD et XDR sont pris en charge) peut être implicite ou explicite (autrement dit, un code de mise à jour peut fonctionner avec ou sans schéma de mappage spécifié). Si vous ne spécifiez pas un schéma de mappage, la mise à jour suppose un mappage implicite (le mappage par défaut), où chaque élément dans le  **\<avant >** bloc ou  **\<après >** bloc mappe à une table et chaque élément enfant d’ou d’attribut est mappé à une colonne dans la base de données. Si vous spécifiez explicitement un schéma de mappage, les éléments et les attributs dans le code de mise à jour doivent correspondre aux éléments et aux attributs dans le schéma de mappage.  
  
### <a name="implicit-default-mapping"></a>Mappage implicite (par défaut)  
 Dans la plupart des cas, un code de mise à jour qui effectue des mises à jour simples peut ne pas requérir de schéma de mappage. Dans ce cas, le code de mise à jour compte sur le schéma de mappage par défaut.  
  
 Le code de mise à jour ci-dessous illustre un mappage implicite. Dans cet exemple, le code de mise à jour insère un nouveau client dans la table Sales.Customer. Étant donné que cette mise à jour utilise un mappage implicite, le \<Sales.Customer > élément est mappé à la table Sales.Customer, et les attributs CustomerID et SalesPersonID sont mappés aux colonnes correspondantes dans la table Sales.Customer.  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
<updg:sync >  
<updg:before>  
</updg:before>  
<updg:after>  
    <Sales.Customer CustomerID="1" SalesPersonID="277" />  
    </updg:after>  
</updg:sync>  
</ROOT>  
```  
  
### <a name="explicit-mapping"></a>Mappage explicite  
 Si vous spécifiez un schéma de mappage (XSD ou XDR), le code de mise à jour utilise le schéma pour déterminer les tables et colonnes de base de données qui seront mises à jour.  
  
 Si la mise à jour effectue une mise à jour complexe (par exemple, insertion d’enregistrements dans plusieurs tables en fonction de la relation parent-enfant qui est spécifié dans le schéma de mappage), vous devez fournir explicitement le schéma de mappage à l’aide de la **schéma de mappage** d’attribut sur lequel s’exécute la mise à jour.  
  
 Comme un code de mise à jour est un modèle, le chemin d'accès spécifié pour le schéma de mappage dans le code de mise à jour est relatif à l'emplacement du fichier modèle (relatif à l'emplacement où le code de mise à jour est stocké). Pour plus d’informations, consultez [spécifiant un schéma de mappage annoté dans une mise à jour &#40;SQLXML 4.0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/updategrams/specifying-an-annotated-mapping-schema-in-an-updategram-sqlxml-4-0.md).  
  
## <a name="element-centric-and-attribute-centric-mapping-in-updategrams"></a>Mappage centré sur les éléments et mappage centré sur les attributs dans les codes de mise à jour  
 Avec le mappage par défaut (lorsque le schéma de mappage n'est pas spécifié dans le code de mise à jour), les éléments de code de mise à jour sont mappés à des tables et les éléments enfants (dans le cas du mappage centré sur les éléments) et les attributs (dans le cas du mappage centré sur les attributs) sont mappés à des colonnes.  
  
### <a name="element-centric-mapping"></a>Mappage centré sur les éléments  
 Dans un code de mise à jour centré sur les éléments, un élément contient des éléments enfants qui désignent les propriétés de l'élément. Pour bénéficier d'un exemple, reportez-vous au code de mise à jour ci-dessous. Le  **\<Person.Contact >** élément contient le  **\<FirstName >** et  **\<LastName >** des éléments enfants. Ces éléments enfants sont des propriétés de la  **\<Person.Contact >** élément.  
  
 Étant donné que cette mise à jour ne spécifie pas un schéma de mappage, la mise à jour utilise un mappage implicite, où le  **\<Person.Contact >** élément est mappé à la table Person.Contact et ses éléments enfants mappent aux colonnes FirstName et LastName.  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
<updg:sync >  
  <updg:after>  
    <Person.Contact>  
       <FirstName>Catherine</FirstName>  
       <LastName>Abel</LastName>  
    </Person.Contact>  
  </updg:after>  
</updg:sync>  
</ROOT>  
```  
  
### <a name="attribute-centric-mapping"></a>mappage centré sur les attributs  
 Dans un mappage centré sur les attributs, les éléments ont des attributs. Le code de mise à jour ci-dessous utilise le mappage centré sur les attributs. Dans cet exemple, le  **\<Person.Contact >** élément comprend la **FirstName** et **LastName** attributs. Ces attributs sont les propriétés de la  **\<Person.Contact >** élément. Comme dans l’exemple précédent, cette mise à jour ne spécifie aucun schéma de mappage, donc elle repose sur un mappage implicite pour mapper le  **\<Person.Contact >** élément à la table Person.Contact et les attributs de l’élément aux colonnes respectives dans la table.  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
<updg:sync >  
  <updg:before>  
  </updg:before>  
  <updg:after>  
    <Person.Contact FirstName="Catherine" LastName="Abel" />  
  </updg:after>  
</updg:sync>  
</ROOT>  
```  
  
### <a name="using-both-element-centric-and-attribute-centric-mapping"></a>Utilisation du mappage centré sur les éléments et du mappage centré sur les attributs  
 Vous pouvez spécifier une association du mappage centré sur les éléments et du mappage centré sur les attributs, comme l'illustre le code de mise à jour ci-dessous. Notez que la  **\<Person.Contact >** élément contient un attribut et un élément enfant. Également, ce code de mise à jour s'appuie sur un mappage implicite. Par conséquent, le **FirstName** attribut et la  **\<LastName >** sont mappés aux colonnes correspondantes dans la table Person.Contact.  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
<updg:sync >  
  <updg:before>  
  </updg:before>  
  <updg:after>  
    <Person.Contact FirstName="Catherine" >  
       <LastName>Abel</LastName>  
    </Person.Contact>  
  </updg:after>  
</updg:sync>  
</ROOT>  
```  
  
## <a name="working-with-characters-valid-in-sql-server-but-not-valid-in-xml"></a>Utilisation des caractères valides dans SQL Server mais non valides dans XML  
 Dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], les noms des tables peuvent inclure un espace. Toutefois, ce type de nom de table n'est pas valide dans XML.  
  
 Pour encoder des caractères valides [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] identificateurs, mais ne sont pas des identificateurs XML valides, utilisez ' __xHHHH\_\_' comme valeur d’encodage, où HHHH représente le code de UCS-2 hexadécimal à quatre chiffres du caractère dans l’ordre du premier bit plus significatif. À l’aide de ce schéma d’encodage, un espace est remplacé par x0020 (le quatre chiffres code hexadécimal d’un espace) ; Par conséquent, le nom de table [Order Details] dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] devient _x005B_Order_x0020_Details_x005D\_ dans XML.  
  
 De même, vous devrez peut-être spécifier des noms d’élément de trois parties, telles que \<[database]. [ Owner]. [table] >. Étant donné que les crochets ([et]) ne sont pas valide dans XML, vous devez spécifier en tant que \<_x005B_database_x005D\_._x005B_owner_x005D\_._x005B_table_x005D\_>, où _x005B\_ l’encodage du crochet gauche ([) et _x005D\_ l’encodage du crochet droit (]).  
  
## <a name="executing-updategrams"></a>Exécution de codes de mise à jour  
 Comme un code de mise à jour est un modèle, tous les mécanismes de traitement d'un modèle s'appliquent aux codes de mise à jour. Pour SQLXML 4.0, vous pouvez exécuter un code de mise à jour d'une des façons suivantes :  
  
-   En le soumettant dans une commande ADO.  
  
-   En le soumettant sous la forme d'une commande OLE DB.  
  
## <a name="see-also"></a>Voir aussi  
 [Considérations de sécurité de mise à jour &#40;SQLXML 4.0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/security/updategram-security-considerations-sqlxml-4-0.md)  
  
  
