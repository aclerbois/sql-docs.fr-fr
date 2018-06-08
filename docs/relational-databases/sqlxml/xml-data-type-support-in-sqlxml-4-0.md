---
title: prise en charge du Type de données dans SQLXML 4.0 XML | Documents Microsoft
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
- SQLXML, xml data type support
- xml data type [SQL Server], SQLXML
ms.assetid: 9a6f5ad8-4a8f-4de7-ac17-81d5ccf78459
caps.latest.revision: 17
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 68cb00ced8f77b5921be07a3f6383d4436f0bade
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="xml-data-type-support-in-sqlxml-40"></a>Prise en charge du type de données xml dans SQLXML 4.0
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  À partir de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)], [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend en charge XML typées de données à l’aide du **xml** type de données. Cette rubrique fournit des informations sur la façon dont SQLXML 4.0 reconnaît les instances de la **xml** type de données et implémente leur prise en charge.  
  
## <a name="working-with-xml-data-types"></a>Utilisation des types de données xml  
 Pour en savoir plus sur l’utilisation des tables SQL qui implémentent **xml** de type de données des colonnes, les exemples suivants sont fournis :  
  
|Tâche|Exemple|Rubrique|  
|----------|-------------|-----------|  
|Comment mapper et inclure une **xml** colonne dans une vue XML|« Mappage d'un élément XML à une colonne de type de données XML »|[Mappage par défaut des éléments XSD et des attributs aux Tables et colonnes &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/default-mapping-of-xsd-elements-and-attributes-to-tables-and-columns-sqlxml-4-0.md)|  
|Comment insérer des données dans un **xml** colonne de codes|« Insertion de données dans une colonne de type de données XML »|[Insertion de données à l’aide de XML Updategrams &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/updategrams/inserting-data-using-xml-updategrams-sqlxml-4-0.md)|  
|Chargement des données XML dans en bloc un **xml** colonne|« Chargement en masse dans les colonnes de type de données xml »|[Exemples de chargement en masse XML & #40 ; SQLXML 4.0 & #41 ;](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/bulk-load-xml/xml-bulk-load-examples-sqlxml-4-0.md)|  
  
## <a name="guidelines-and-limitations"></a>Instructions et limitations  
  
-   **\<XSD : n’importe quel >** ne peut pas être mappé à une colonne, y compris un **xml** type de données. Prise en charge dans SQLXML pour ce scénario est fourni par le **SQL : Overflow-champ** annotation. Une autre solution consiste à mapper un **xml** en tant qu’élément de champ de type **xsd : anyType**. Cette solution est illustrée dans l'exemple de mappage d'un élément XML à une colonne de type de données XML, référencé dans le tableau ci-dessus.  
  
-   Requête XPath dans le contenu du **xml** les colonnes de type de données n’est pas pris en charge.  
  
-   À l’aide une **xml** colonne de type de données dans les annotations où il n'est pas pris en charge (tels que **SQL : Relationship** et **SQL : Key-champs**) ou autorisé, provoque des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] erreurs qui ne sont pas interceptées par les composants de couche intermédiaire implémentant SQLXML 4.0. Cela se produit, car SQLXML ne requiert pas d'informations de type SQL. Ce comportement est semblable à celui de SQLXML pour les autres types de données, par exemple les types BLOB et binaires.  
  
-   Mappage **xml** colonnes est pris en charge uniquement pour les schémas XSD. Schémas XDR ne prennent pas en charge le mappage **xml** colonnes.  
  
-   SQLXML 4.0 s'appuie sur la prise en charge de l'analyse XML fournie dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Un **xml** colonne peut être mappée en tant que données XML typées ou XML non typé. Dans les deux cas, SQLXML 4.0 ne valide pas le code XML d'entrée.  Si le code XML d'entrée n'est pas valide ou s'il est incorrect, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] le signale à SQLXML et propage toutes les informations d'erreur pertinentes retournées par le serveur à l'utilisateur.  
  
-   SQLXML 4,0 s'appuie sur la prise en charge limitée des DTD a fourni dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] permet une DTD interne dans **xml** type de données, qui peut être utilisé pour fournir des valeurs par défaut et remplacez les références d’entité par leur contenu développé. SQLXML passe les données XML « en l'état » (y compris la DTD interne) au serveur. Vous pouvez convertir les DTD en documents XSD (XML Schema Documents) à l'aide d'outils tiers, puis charger les données avec des schémas XSD insérés dans la base de données.  
  
-   SQLXML 4.0 ne conserve pas les instructions de traitement de déclaration XML (par exemple), en fonction du comportement de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Au lieu de cela, la déclaration XML est traitée comme une directive pour le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] analyseur XML et ses attributs (version, le codage et autonome) sont perdus une fois que les données sont converties dans le **xml** type de données. Les données XML sont stockées en interne au format UCS-2. Toutes les autres instructions de traitement dans l’instance XML sont conservées ; ils sont autorisés dans les **xml** colonne et peut être pris en charge par SQLXML.  
  
## <a name="see-also"></a>Voir aussi  
 [Données XML &#40;SQL Server&#41;](../../relational-databases/xml/xml-data-sql-server.md)  
  
  
