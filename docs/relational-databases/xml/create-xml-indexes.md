---
title: Créer des index XML | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: xml
ms.reviewer: ''
ms.suite: sql
ms.technology: xml
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- indexes [XML in SQL Server]
- XML indexes [SQL Server], creating
ms.assetid: 6ecac598-355d-4408-baf7-1b2e8d4cf7c1
caps.latest.revision: 19
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 15c81de1d71da6f0773c53833c2849fd58d1f3ce
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-xml-indexes"></a>Créer des index XML
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]
  Cette rubrique décrit comment créer des index XML primaires et secondaires.  
  
## <a name="creating-a-primary-xml-index"></a>Création d'un index XML primaire  
 Pour créer un index XML primaire, utilisez l’instruction DDL [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)[!INCLUDE[tsql](../../includes/tsql-md.md)]. Les options disponibles pour les index non XML ne sont pas toutes prises en charge pour les index XML.  
  
 Lorsque vous créez un index XML, prenez les points suivants en considération :  
  
-   Pour créer un index XML primaire, la table contenant la colonne XML en cours d'indexation, appelée table de base, doit posséder un index cluster portant sur la clé primaire. Ceci permet de s'assurer que, si la table de base est partitionnée, l'index XML primaire peut être partitionné grâce au même schéma de partitionnement et à la même fonction de partitionnement.  
  
-   Si un index XML existe, la clé primaire mise en cluster de la table ne peut alors pas être modifiée. Vous devez dans ce cas supprimer tous les index XML de la table avant de pouvoir modifier la clé primaire.  
  
-   Un index XML primaire portant sur une seule colonne de type **xml** peut être créé. Aucun autre type d'index relatif à la colonne de type XML ne peut être créé en tant que colonne clé. Vous pouvez cependant inclure la colonne de type **xml** dans un index non XML. Chaque colonne de type **xml** d’une table peut présenter son propre index XML primaire. Ceci dit, un seul index XML primaire est autorisé par colonne de type **xml** .  
  
-   Les index XML existent dans le même espace de noms que les index non XML. Vous ne pouvez par conséquent pas avoir un index XML et un index non XML portant tous deux le même nom pour une même table.  
  
-   Les options IGNORE_DUP_KEY et ONLINE sont toujours définies sur OFF (c'est-à-dire désactivées) pour les index XML. Vous pouvez indiquer la valeur OFF pour ces options.  
  
-   Les informations de groupes de fichiers ou de partitionnement de la table utilisateur s'appliquent à l'index XML. Les utilisateurs ne peuvent pas indiquer ces informations séparément pour un index XML.  
  
-   L'option d'index DROP_EXISTING peut supprimer un index XML primaire et en recréer un, ou supprimer un index XML secondaire et en recréer un. Elle ne peut cependant pas entraîner la suppression d'index XML secondaire pour créer un index XML primaire ou vice versa.  
  
-   Les noms s'appliquant aux index XML primaires sont soumis aux mêmes restrictions que les noms de vues.  
  
 Vous ne pouvez donc pas créer d’index XML portant sur une colonne de type **xml** dans une vue, sur une variable affectée d’une valeur de type **table** et possédant des colonnes de type **xml** ou encore des variables de type **xml** .  
  
-   Pour modifier une colonne de type **xml** non typé en XML typé ou vice versa par le biais de l’option ALTER TABLE ALTER COLUMN, assurez-vous qu’aucun index XML portant sur la colonne n’existe. Si c'est le cas, il doit être supprimé avant de pouvoir tenter de modifier le type de colonne.  
  
-   L'option ARITHABORT doit être activée (c'est-à-dire définie sur ON) lors de la création d'un index XML. Pour pouvoir interroger, insérer, supprimer ou mettre à jour des valeurs de la colonne XML par le biais des méthodes portant sur des données de type XML, cette même option doit être définie sur la connexion aux données. Dans le cas contraire, les méthodes portant sur les données de type XML échouent.  
  
    > [!NOTE]  
    >  Des informations à propos d'un index XML donné sont répertoriées dans les affichages catalogue. La procédure **sp_helpindex** n’est cependant pas prise en charge. Des exemples fournis plus loin dans cette rubrique illustrent comment lancer une requête sur les affichages catalogue afin de retrouver les informations propres aux index XML.  
  
 Lors de la création ou de la recréation d’un index XML primaire sur une colonne de type de données XML qui contient des valeurs des types de schémas XML **xs:date** ou **xs:dateTime** (ou tout sous-type de ces types) dont l’année est inférieure à 1, la création de l’index échoue dans [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et les versions ultérieures. [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] Ces valeurs étaient autorisées, ce problème peut donc se produire lors de la création d’index dans une base de données générée dans [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]. Pour plus d’informations, consultez [Comparer du XML typé et du XML non typé](../../relational-databases/xml/compare-typed-xml-to-untyped-xml.md).  
  
### <a name="example-creating-a-primary-xml-index"></a>Exemple : création d'un index XML primaire  
 La table T (pk INT PRIMARY KEY, xCol XML) avec une colonne XML non typée est utilisée dans la plupart des exemples. Cette syntaxe peut très aisément s'adapter à du code XML typé. Par souci de clarté, les requêtes sont décrites pour des instances de données XML, comme le montre l'exemple qui suit :  
  
```  
<book genre="security" publicationdate="2002" ISBN="0-7356-1588-2">  
   <title>Writing Secure Code</title>  
   <author>  
      <first-name>Michael</first-name>  
      <last-name>Howard</last-name>  
   </author>  
   <author>  
      <first-name>David</first-name>  
      <last-name>LeBlanc</last-name>  
   </author>  
   <price>39.99</price>  
</book>  
```  
  
 L'instruction suivante crée un index XML, appelé idx_xCol, sur la colonne XML xCol de la table T :  
  
```  
CREATE PRIMARY XML INDEX idx_xCol on T (xCol)  
```  
  
## <a name="creating-a-secondary-xml-index"></a>Création d'un index XML secondaire  
 Utilisez l’instruction DDL [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)[!INCLUDE[tsql](../../includes/tsql-md.md)] pour créer des index XML secondaires et préciser leur type.  
  
 Lorsque vous créez un index XML secondaire, prenez les points suivants en considération :  
  
-   Toutes les options d'indexation qui s'appliquent à un index non-cluster, hormis les options IGNORE_DUP_KEY et ONLINE, sont autorisées sur les index XML secondaires. Ces deux options doivent toujours être définies sur OFF dans le cas d'index XML secondaires.  
  
-   Les index secondaires sont partitionnés de la même manière que l'index XML primaire.  
  
-   DROP_EXISTING permet de supprimer un index secondaire sur la table utilisateur pour en recréer un.  
  
 Vous pouvez passer par une requête sur l’affichage catalogue **sys.xml_indexes** afin d’extraire les informations relatives aux index XML. Il reste à noter que la colonne **secondary_type_desc** se trouvant dans l’affichage catalogue **sys.xml_indexes** fournit le type d’index secondaire :  
  
```  
SELECT  *   
FROM    sys.xml_indexes;  
```  
  
 Les valeurs retournées dans la colonne **secondary_type_desc** peuvent être NULL, PATH, VALUE ou PROPERTY. Pour l'index XML primaire, la valeur renvoyée correspond à NULL.  
  
### <a name="example-creating-secondary-xml-indexes"></a>Exemple: création d'index XML secondaires  
 L'exemple suivant illustre le mode de création d'index XML secondaires. Il montre également les informations relatives aux index XML que vous avez créés.  
  
```  
CREATE TABLE T (Col1 INT PRIMARY KEY, XmlCol XML);  
GO  
-- Create primary index.  
CREATE PRIMARY XML INDEX PIdx_T_XmlCol   
ON T(XmlCol);  
GO  
-- Create secondary indexes (PATH, VALUE, PROPERTY).  
CREATE XML INDEX PIdx_T_XmlCol_PATH ON T(XmlCol)  
USING XML INDEX PIdx_T_XmlCol  
FOR PATH;  
GO  
CREATE XML INDEX PIdx_T_XmlCol_VALUE ON T(XmlCol)  
USING XML INDEX PIdx_T_XmlCol  
FOR VALUE;  
GO  
CREATE XML INDEX PIdx_T_XmlCol_PROPERTY ON T(XmlCol)  
USING XML INDEX PIdx_T_XmlCol  
FOR PROPERTY;  
GO  
```  
  
 Vous pouvez interroger le `sys.xml_indexes` afin d'extraire des informations relatives aux index XML. La colonne `secondary_type_desc` fournit le type d'index secondaire.  
  
```  
SELECT  *   
FROM    sys.xml_indexes;  
```  
  
 Vous pouvez également interroger l'affichage catalogue pour obtenir les informations sur l'index.  
  
```  
SELECT *  
FROM sys.xml_indexes  
WHERE object_id = object_id('T');  
```  
  
 Vous pouvez ajouter des exemples de données puis passer en revue les informations de l'index XML.  
  
```  
INSERT INTO T VALUES (1,  
'<doc id="123">  
<sections>  
<section num="2">  
<heading>Background</heading>  
</section>  
<section num="3">  
<heading>Sort</heading>  
</section>  
<section num="4">  
<heading>Search</heading>  
</section>  
</sections>  
</doc>');  
GO  
-- Check XML index information.  
SELECT *  
FROM   sys.dm_db_index_physical_stats (db_id(), object_id('T'), NULL, NULL, 'DETAILED');  
GO  
-- Space usage of primary XML index  
DECLARE @index_id int;  
SELECT  @index_id = i.index_id  
FROM    sys.xml_indexes i   
WHERE   i.name = 'PIdx_T_XmlCol' and object_name(i.object_id) = 'T';  
  
SELECT *  
FROM sys.dm_db_index_physical_stats (db_id(), object_id('T') , @index_id, DEFAULT, 'DETAILED');  
go  
--- Space usage of secondary XML index (for example PATH secondary index)  PIdx_T_XmlCol_PATH  
DECLARE @index_id int;  
SELECT  @index_id = i.index_id   
FROM    sys.xml_indexes i   
WHERE  i.name = 'PIdx_T_XmlCol_PATH' and object_name(i.object_id) = 'T';  
  
SELECT *  
FROM sys.dm_db_index_physical_stats (db_id(), object_id('T') , @index_id, DEFAULT, 'DETAILED');  
go  
  
-- Space usage of all secondary XML indexes for a particular table  
SELECT i.name, object_name(i.object_id), stats.*   
FROM   sys.dm_db_index_physical_stats (db_id(), object_id('T'), NULL, DEFAULT, 'DETAILED') stats  
JOIN sys.xml_indexes i ON (stats.object_id = i.object_id and stats.index_id = i.index_id)  
WHERE secondary_type is not null;  
-- Drop secondary indexes.  
DROP INDEX PIdx_T_XmlCol_PATH ON T;  
GO  
DROP INDEX PIdx_T_XmlCol_VALUE ON T;  
GO  
DROP INDEX PIdx_T_XmlCol_PROPERTY ON T;  
GO  
-- Drop primary index.  
DROP INDEX PIdx_T_XmlCol ON T;  
-- Drop table T.  
DROP TABLE T;  
Go  
```  
  
## <a name="see-also"></a> Voir aussi  
 [Index XML &#40;SQL Server&#41;](../../relational-databases/xml/xml-indexes-sql-server.md)   
 [Données XML &#40;SQL Server&#41;](../../relational-databases/xml/xml-data-sql-server.md)  
  
  
