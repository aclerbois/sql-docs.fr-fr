---
title: Comparer du XML typé et du XML non typé | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: xml
ms.reviewer: ''
ms.suite: sql
ms.technology: xml
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- xml data type [SQL Server], variables
- parameters [XML in SQL Server]
- facets [XML in SQL Server]
- xml data type [SQL Server], columns
- untyped XML
- xml data type [SQL Server], typed xml
- XML [SQL Server], typed
- variables [XML in SQL Server], creating
- xml data type [SQL Server], untyped xml
- columns [XML in SQL Server], creating
- typed XML
- document mode processing [SQL Server]
- XML [SQL Server], untyped
- xml data type [SQL Server], parameters
ms.assetid: 4bc50af9-2f7d-49df-bb01-854d080c72c7
caps.latest.revision: 57
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: b5727a822ade990d730642f4413cbdead5ffdb64
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="compare-typed-xml-to-untyped-xml"></a>Comparer du XML typé et du XML non typé
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]
  Vous pouvez créer des variables, des paramètres et des colonnes du type **xml** . Vous pouvez éventuellement associer une collection de schémas XML à une variable, un paramètre ou une colonne de type **xml** . Dans ce cas, l'instance de type de données **xml** est dite *typée*. Dans le cas contraire, l'instance XML est dite *non typée*.  
  
## <a name="well-formed-xml-and-the-xml-data-type"></a>Code XML bien formé et le type de données xml  
 Le type de données **xml** implémente le type de données **xml** de norme ISO. Par conséquent, il peut stocker des documents bien formés en conformité avec XML version 1.0, ainsi que des fragments de contenu XML avec des nœuds de texte et un nombre arbitraire d'éléments de premier niveau dans une colonne XML non typée. Le système vérifie que les données sont bien formées, n'exige pas l'association de schémas XML avec la colonne, puis rejette les données mal formées au sens large. Il peut aussi s'agir de variables et de paramètres XML non typés.  
  
## <a name="xml-schemas"></a>Schémas XML  
 Un schéma XML donne les informations suivantes :  
  
-   **Contraintes de validation.** Chaque fois qu'une instance typée xml est affectée ou modifiée, SQL Server la valide.  
  
-   **Informations sur le type de donnes.** Les schémas fournissent des informations sur les types des attributs et des éléments de l’instance de type de données **xml** . Les informations de type fournissent une sémantique opérationnelle plus précise aux valeurs contenues dans l'instance qu'il n'est possible avec **xml**non typé. Par exemple, des opérations arithmétiques décimales peuvent être effectuées sur une valeur décimale, mais pas sur une chaîne. Grâce à cela, le stockage des données typées XML est considérablement plus compact qu'en cas de code XML non typé.  
  
## <a name="choosing-typed-or-untyped-xml"></a>Choix de XML typé ou non typé  
 Utilisez le type de données **xml** non typé dans les cas suivants :  
  
-   Vous n'avez pas de schéma pour vos données XML.  
  
-   Vous avez des schémas, mais vous ne voulez pas que le serveur valide les données. C'est notamment le cas lorsqu'une application effectue la validation côté client avant de stocker les données sur le serveur, ou stocke temporairement les données XML déclarées non valides par rapport au schéma, ou utilise des composants de schéma non pris en charge par le serveur.  
  
 Utilisez le type de données **xml** typé dans les cas suivants :  
  
-   Il existe des schémas pour vos données XML et vous souhaitez que le serveur valide les données XML par rapport aux schémas XML.  
  
-   Vous voulez profiter de l'optimisation du stockage et des requêtes que permettent les informations de type.  
  
-   Vous voulez tirer parti des avantages que procurent les informations de type lors de la compilation de vos requêtes.  
  
 Des colonnes, paramètres et variables en XML typé peuvent stocker des documents ou du contenu XML. Toutefois, vous devez utiliser un indicateur pour spécifier qu'il s'agit d'un document ou d'un contenu au moment de la déclaration. De plus, vous devez fournir la collection de schémas XML. Spécifiez DOCUMENT si chaque instance XML a un seul et unique élément de premier niveau. Sinon, utilisez CONTENT. Le compilateur de requête utilise l'indicateur DOCUMENT dans les vérifications de type lors de la compilation de la requête pour déduire les éléments singletons de premier niveau.  
  
## <a name="creating-typed-xml"></a>Création de XML typé  
 Avant de créer des variables, des paramètres ou des colonnes **xml** typés, vous devez inscrire la collection de schémas XML à l’aide de [CREATE XML SCHEMA COLLECTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-xml-schema-collection-transact-sql.md). Vous pouvez ensuite associer la collection de schémas XML aux variables, paramètres ou colonnes typés **xml** .  
  
 Dans les exemples suivants, une convention d'affectation des noms en deux parties est utilisée pour spécifier le nom de la collection de schémas XML. La première partie est le nom du schéma et la deuxième est le nom de la collection de schémas XML.  
  
### <a name="example-associating-a-schema-collection-with-an-xml-type-variable"></a>Exemple : association d'une collection de schémas à une variable de type xml  
 L’exemple suivant crée une variable de type **xml** et lui associe une collection de schémas. La collection de schémas spécifiée dans l'exemple est déjà importée dans la base de données **AdventureWorks** .  
  
```  
DECLARE @x xml (Production.ProductDescriptionSchemaCollection);   
```  
  
### <a name="example-specifying-a-schema-for-an-xml-type-column"></a>Exemple: spécification d'un schéma pour une colonne de type xml  
 L'exemple suivant crée une table avec une colonne de type **xml** et spécifie le schéma correspondant :  
  
```  
CREATE TABLE T1(  
 Col1 int,   
 Col2 xml (Production.ProductDescriptionSchemaCollection)) ;  
```  
  
### <a name="example-passing-an-xml-type-parameter-to-a-stored-procedure"></a>Exemple : transmission d'un paramètre de type xml vers une procédure stockée  
 L'exemple suivant passe un paramètre de type **xml** à une procédure stockée et spécifie un schéma pour la variable :  
  
```  
CREATE PROCEDURE SampleProc   
  @ProdDescription xml (Production.ProductDescriptionSchemaCollection)   
AS   
...  
```  
  
 Notez les points suivants à propos de la collection de schémas XML :  
  
-   Une collection de schémas XML est disponible seulement dans la base de données où elle a été inscrite à l'aide de [CREATE XML SCHEMA COLLECTION](../../t-sql/statements/create-xml-schema-collection-transact-sql.md).  
  
-   Si vous transformez une chaîne en données typées **xml** , l'analyse se charge de la validation et de l'attribution du type en fonction des espaces de noms du schéma XML de la collection spécifiée.  
  
-   Vous pouvez convertir des données de **xml** typé en **xml** non typé, et inversement.  
  
 Pour plus d’informations sur d’autres façons de générer du code XML dans SQL Server, consultez [Créer des instances de données XML](../../relational-databases/xml/create-instances-of-xml-data.md). Une fois le code XML généré, il peut être affecté à une variable de type **xml** ou stocké dans des colonnes de type **xml** en vue d'un traitement supplémentaire.  
  
 Dans la hiérarchie des types de données, le type de données **xml** apparaît après **sql_variant** et les types définis par l’utilisateur, mais avant tout type intégré.  
  
### <a name="example-specifying-facets-to-constrain-a-typed-xml-column"></a>Exemple : définition de facettes pour imposer des contraintes sur une colonne typée xml  
 Vous pouvez définir des contraintes sur des colonnes **xml** typées de façon à n’autoriser que des éléments uniques de premier niveau pour chaque instance qui y est stockée. Pour cela, vous devez spécifier la facette facultative `DOCUMENT` lors de la création de la table, comme le montre l'exemple suivant :  
  
```  
CREATE TABLE T(Col1 xml   
   (DOCUMENT Production.ProductDescriptionSchemaCollection));  
GO  
DROP TABLE T;  
GO  
```  
  
 Par défaut, les instances stockées dans la colonne typée **xml** sont considérées comme du contenu XML et non comme des documents XML de façon à autoriser :  
  
-   zéro ou plusieurs éléments de premier niveau ;  
  
-   des nœuds de texte dans les éléments de premier niveau.  
  
 Vous pouvez aussi définir ce comportement de manière explicite en ajoutant la facette `CONTENT` , conformément à l'exemple suivant :  
  
```  
CREATE TABLE T(Col1 xml(CONTENT Production.ProductDescriptionSchemaCollection));  
GO -- Default  
```  
  
 Notez que vous pouvez spécifier les facettes facultatives DOCUMENT/CONTENT partout où vous définissez le type **xml** (xml typé). Par exemple, quand vous créez une variable **xml** typée, vous pouvez ajouter la facette DOCUMENT/CONTENT, comme illustré dans l’exemple suivant :  
  
```  
declare @x xml (DOCUMENT Production.ProductDescriptionSchemaCollection);  
```  
  
## <a name="document-type-definition-dtd"></a>Définition de type de document (DTD)  
 Les colonnes, variables et paramètres de type de données **xml** peuvent être typés à l'aide d'un schéma XML, mais pas en utilisant DTD. Toutefois, une DTD en ligne peut être utilisée à la fois avec du code XML typé et non typé pour fournir des valeurs par défaut et remplacer des références d'entité par leur forme étendue.  
  
 Vous pouvez convertir les DTD en documents de schéma XML à l'aide d'outils tiers, puis charger les schémas XML dans la base de données.  
  
## <a name="upgrading-typed-xml-from-sql-server-2005"></a>Mise à niveau de XML typé à partir de SQL Server 2005  
 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] offre plusieurs extensions de prise en charge du schéma XML, y compris la prise en charge de la validation de type lax, une gestion améliorée des données d'instance **xs:date**, **xs:time** et **xs:dateTime** et la nouvelle prise en charge des types de liste et d'union. Dans la plupart des cas, les modifications n'affectent pas l'expérience de mise à niveau. Toutefois, si vous avez utilisé une collection de schémas XML dans [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] qui autorisait les valeurs de type **xs:date**, **xs:time**ou **xs:dateTime** (ou n’importe quel sous-type), les étapes de mise à niveau suivantes ont lieu quand vous joignez votre base de données [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] à une version ultérieure de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]:  
  
1.  Pour chaque colonne XML qui est typée avec une collection de schémas XML contenant des éléments ou des attributs typés comme **xs:anyType**, **xs:anySimpleType**, **xs:date** ou l'un de ses sous-types, **xs:time** ou tout sous-type, ou **xs:dateTime** ou l'un de ses sous-types, ou qui constituent des types d'union ou de liste contenant l'un de ces types, les événements suivants ont lieu :  
  
    1.  Tous les index XML sur la colonne seront désactivés.  
  
    2.  Toutes les valeurs [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] continueront d'être représentées dans la période Z, car elles ont été normalisées selon cette période.  
  
    3.  Toute valeur **xs:date** ou **xs:dateTime** plus petite que le 1er janvier de l’année 1 provoque une erreur d’exécution quand l’index est reconstruit ou que des instructions XQuery ou XML-DML sont exécutées sur le type de données XML contenant cette valeur.  
  
2.  Toute année négative dans les facettes **xs:date** ou **xs:dateTime** ou les valeurs par défaut dans une collection de schémas XML sont mises à jour automatiquement en fonction de la plus petite valeur autorisée par le type **xs:date** ou **xs:dateTime** de base (par exemple, 0001-01-01T00:00:00.0000000Z pour **xs:dateTime**).  
  
 Notez que vous pouvez encore utiliser une simple instruction select SQL pour extraire le type de données XML entier, même s'il contient des années négatives. Il est recommandé de remplacer les années négatives par une année comprise dans la plage nouvellement prise en charge, ou de modifier le type de l'élément ou attribut en **xs:string**.  
  
## <a name="see-also"></a> Voir aussi  
 [Créer des instances de données XML](../../relational-databases/xml/create-instances-of-xml-data.md)   
 [méthodes de type de données xml](../../t-sql/xml/xml-data-type-methods.md)   
 [Langage de modification de données XML &#40;XML DML&#41;](../../t-sql/xml/xml-data-modification-language-xml-dml.md)   
 [Données XML &#40;SQL Server&#41;](../../relational-databases/xml/xml-data-sql-server.md)  
  
  
