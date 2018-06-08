---
title: Contexte de l’expression et l’évaluation de la requête (XQuery) | Documents Microsoft
ms.custom: ''
ms.date: 03/17/2017
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
- expression context [XQuery]
- XQuery, expression context
- XQuery, query evaluation
- static context
- dynamic context [XQuery]
ms.assetid: 5059f858-086a-40d4-811e-81fedaa18b06
caps.latest.revision: 19
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: 4b9100ff14fc4cbd5e7d2a94830741fa6a1ceda2
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="expression-context-and-query-evaluation-xquery"></a>Contexte des expressions et évaluation des requêtes (XQuery)
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

  Le contexte d'une expression rassemble les informations utilisées pour l'analyser et l'évaluer. Voici les deux phases au cours desquelles XQuery est évalué :  
  
-   **Contexte statique** : c’est la phase de compilation de requête. En fonction des informations disponibles, des erreurs sont quelquefois générées au cours de l'analyse statique de la requête.  
  
-   **Contexte dynamique** – il s’agit la phase d’exécution de requête. Même si elle ne présente pas d'erreurs statiques (erreurs survenues lors de la compilation de la requête), la requête peut renvoyer des erreurs au cours de son exécution.  
  
## <a name="static-context"></a>Contexte statique  
 L'initialisation du contexte statique consiste à rassembler toutes les informations requises par l'analyse statique de l'expression. L'initialisation du contexte statique comprend les étapes suivantes :  
  
-   Le **espace limite** stratégie est définie sur strip. Par conséquent, l’espace limite n’est pas conservé par le **n’importe quel élément** et **attribut** constructeurs dans la requête. Par exemple :  
  
    ```  
    declare @x xml  
    set @x=''  
    select @x.query('<a>  {"Hello"}  </a>,  
  
        <b> {"Hello2"}  </b>')  
    ```  
  
     Cette requête renvoie le résultat suivant puisque l'espace limite est ignoré lors de l'analyse de l'expression XQuery :  
  
    ```  
    <a>Hello</a><b>Hello2</b>  
    ```  
  
-   Le préfixe et l'espace de noms de liaison sont initialisés pour les éléments suivants :  
  
    -   Un ensemble d'espaces de noms prédéfinis.  
  
    -   Tous les espaces de noms définis à l'aide de WITH XMLNAMESPACES. Pour plus d’informations, consultez [espaces de noms à ajouter à des requêtes avec WITH XMLNAMESPACES](../relational-databases/xml/add-namespaces-to-queries-with-with-xmlnamespaces.md)).  
  
    -   Tous les espaces de noms définis dans le prologue de la requête. Remarquez que les déclarations d'espace de noms du prologue peuvent remplacer la déclaration d'espace de noms de WITH XMLNAMESPACES. Par exemple, dans la requête suivante, WITH XMLNAMESPACES déclare un préfixe (pd) qui lie à l’espace de noms (`http://someURI`). En revanche, dans la clause WHERE, le prologue de la requête remplace la liaison.  
  
        ```  
        WITH XMLNAMESPACES ('http://someURI' AS pd)  
        SELECT ProductModelID, CatalogDescription.query('  
            <Product   
                ProductModelID= "{ sql:column("ProductModelID") }"   
                />  
        ') AS Result  
        FROM Production.ProductModel  
        WHERE CatalogDescription.exist('  
            declare namespace  pd="http://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription";  
             /pd:ProductDescription[(pd:Specifications)]'  
            ) = 1  
        ```  
  
     Toutes les liaisons d'espace de noms sont résolues au cours de l'initialisation du contexte statique.  
  
-   Si l’interrogation typé **xml** colonne ou une variable, les composants de la collection de schémas XML associé à la colonne ou la variable sont importés dans le contexte statique. Pour plus d’informations, consultez [Comparer du XML typé et du XML non typé](../relational-databases/xml/compare-typed-xml-to-untyped-xml.md).  
  
-   Pour chaque type atomique des schémas importés, une fonction de conversion est également mise à disposition dans le contexte statique. L'exemple suivant illustre ce concept. Dans cet exemple, une requête est spécifiée contre un typé **xml** variable. La collection de schémas XML associée à cette variable définit un type atomique, myType. Correspondant à ce type, une fonction de conversion, **myType()**, lors de l’analyse statique n’est disponible. L'expression de la requête (`ns:myType(0)`) renvoie une valeur de type myType.  
  
    ```  
    -- DROP XML SCHEMA COLLECTION SC  
    -- go  
    CREATE XML SCHEMA COLLECTION SC AS '<schema xmlns="http://www.w3.org/2001/XMLSchema"   
    targetNamespace="myNS" xmlns:ns="myNS"  
    xmlns:s="http://schemas.microsoft.com/sqlserver/2004/sqltypes">  
          <import namespace="http://schemas.microsoft.com/sqlserver/2004/sqltypes"/>  
          <simpleType name="myType">  
                <restriction base="int">  
                 <enumeration value="0" />  
                  <enumeration value="1"/>  
                </restriction>  
          </simpleType>  
          <element name="root" type="ns:myType"/>  
    </schema>'  
    go  
  
    DECLARE @var XML(SC)  
    SET @var = '<root xmlns="myNS">0</root>'  
    -- specify myType() casting function in the query  
    SELECT @var.query('declare namespace ns="myNS"; ns:myType(0)')  
    ```  
  
     Dans l’exemple suivant, la fonction CAST pour la **int** type XML intégré est spécifiée dans l’expression.  
  
    ```  
    declare @x xml  
    set @x = ''  
    select @x.query('xs:int(5)')  
    go  
    ```  
  
 Après l'initialisation du contexte statique, l'expression de requête est analysée (compilée). L'analyse statique implique les étapes suivantes :  
  
1.  Analyse de la requête.  
  
2.  Résolution des noms de fonction et de type spécifiés dans l'expression.  
  
3.  Typage statique de la requête. De cette façon, le type de la requête est sûr. Par exemple, la requête suivante retourne une erreur statique, car le **+** opérateur requiert des arguments de type primitif numérique :  
  
    ```  
    declare @x xml  
    set @x=''  
    SELECT @x.query('"x" + 4')  
    ```  
  
     Dans l’exemple suivant, la **value()** opérateur nécessite un singleton. Comme indiqué dans le schéma XML, il peut y avoir plusieurs \<Elem > éléments. L'analyse statique de l'expression détermine que le type n'est pas sûr et une erreur statique est générée. Pour corriger cette erreur, vous devez réécrire l'expression de façon à spécifier explicitement un singleton (`data(/x:Elem)[1]`).  
  
    ```  
    DROP XML SCHEMA COLLECTION SC  
    go  
    CREATE XML SCHEMA COLLECTION SC AS '<schema xmlns="http://www.w3.org/2001/XMLSchema"   
    targetNamespace="myNS" xmlns:ns="myNS"  
    xmlns:s="http://schemas.microsoft.com/sqlserver/2004/sqltypes">  
          <import namespace="http://schemas.microsoft.com/sqlserver/2004/sqltypes"/>  
          <element name="Elem" type="string"/>  
    </schema>'  
    go  
  
    declare @x xml (SC)  
    set @x='<Elem xmlns="myNS">test</Elem><Elem xmlns="myNS">test2</Elem>'  
    SELECT @x.value('declare namespace x="myNS"; data(/x:Elem)[1]','varchar(20)')  
    ```  
  
     Pour plus d’informations, consultez [XQuery et le typage statique](../xquery/xquery-and-static-typing.md).  
  
### <a name="implementation-restrictions"></a>Limites de la mise en œuvre  
 Voici les limites liées à un contexte statique :  
  
-   Le mode de compatibilité XPath n'est pas pris en charge.  
  
-   En cas de construction XML, seul le mode de construction strip est pris en charge. Il s'agit du paramètre par défaut. Par conséquent, le type du nœud d’élément construit est de **xdt : non typé** type et les attributs sont des **xdt : untypedAtomic** type.  
  
-   Seul le mode de classement ordered est pris en charge.  
  
-   Seule la stratégie d'espaces XML strip est prise en charge.  
  
-   La fonctionnalité URI de base n'est pas prise en charge.  
  
-   **fn:doc()** n’est pas pris en charge.  
  
-   **fn:collection()** n’est pas pris en charge.  
  
-   XQuery Static Flagger n'est pas fourni.  
  
-   Le classement associé à la **xml** type de données est utilisé. Ce classement est toujours paramétré sur le classement des points de code Unicode.  
  
## <a name="dynamic-context"></a>Contexte dynamique  
 Le contexte dynamique fait référence aux informations dont vous devez disposer au moment de l'exécution de l'expression. Outre le contexte statique, les informations suivantes sont initialisées comme partie du contexte dynamique :  
  
-   Le focus de l'expression tel que l'élément contextuel, la position contextuelle et la taille du contexte, est initialisé comme le montre l'exemple suivant. Notez que toutes ces valeurs peuvent être remplacées par la [méthode nodes()](../t-sql/xml/nodes-method-xml-data-type.md).  
  
    -   Le **xml** type de données définit l’élément de contexte, le nœud en cours de traitement, le nœud de document.  
  
    -   La position contextuelle, c'est-à-dire la position de l'élément contextuel par rapport aux nœuds à traiter, est tout d'abord définie à 1.  
  
    -   La taille du contexte, c'est-à-dire le nombre d'éléments de la séquence à traiter, est tout d'abord définie à 1 puisqu'il y a toujours un seul nœud de document.  
  
### <a name="implementation-restrictions"></a>Limites de la mise en œuvre  
 Voici les limites liées à un contexte dynamique :  
  
-   Le **date et heure actuelles** fonctions relatives au contexte, **fn:current-date**, **fn:current-heure**, et **fn:current-dateTime**, ne sont pas pris en charge.  
  
-   Le **fuseau horaire implicite** est fixé à UTC + 0 et ne peut pas être modifié.  
  
-   Le **fn:doc()** fonction n’est pas prise en charge. Toutes les requêtes sont exécutées sur **xml** variables ou les colonnes de type.  
  
-   Le **fn:collection()** fonction n’est pas prise en charge.  
  
## <a name="see-also"></a>Voir aussi  
 [Principes fondamentaux de XQuery](../xquery/xquery-basics.md)   
 [Comparer du XML typé et du XML non typé](../relational-databases/xml/compare-typed-xml-to-untyped-xml.md)   
 [Collections de schémas XML &#40;SQL Server&#41;](../relational-databases/xml/xml-schema-collections-sql-server.md)  
  
  
