---
title: Utilisation d’Annotations dans les schémas XSD (SQLXML 4.0) | Documents Microsoft
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
- annotated XSD schemas, about annotated XSD schemas
- annotations [SQLXML]
- relationships [SQLXML]
- relationships [SQLXML], hierarchical relationships
- hierarchical relationships [SQLXML]
- mapping schema [SQLXML], scenarios for using
ms.assetid: 78f318a4-7a36-473b-9852-a4bae6940ce5
caps.latest.revision: 26
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: f3e44d91a958441e421259b7257df49f14883eb0
ms.sourcegitcommit: 2d93cd115f52bf3eff3069f28ea866232b4f9f9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34707538"
---
# <a name="using-annotations-in-xsd-schemas-sqlxml-40"></a>Utilisation des annotations dans les schémas XSD (SQLXML 4.0)
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  Dans [!INCLUDE[msCoName](../../includes/msconame-md.md)] SQLXML 4.0, le langage de schéma XSD prend en charge les annotations d'une manière similaire aux annotations introduites avec le langage de schéma XDR (XML-Data Reduced). Des annotations supplémentaires ont été introduites dans XSD qui ne sont pas prises en charge dans XDR.  
  
 Ces annotations peuvent être utilisées dans le schéma XSD pour spécifier le mappage des données XML et des données relationnelles. Est inclus le mappage entre les éléments et les attributs du schéma XSD avec les tables (vues) et les colonnes des bases de données.  
  
 Si vous ne spécifiez pas les annotations, le mappage par défaut a lieu. Par défaut, un élément XSD avec un type complexe est mappé avec un nom de table (vue) de la base de données spécifiée, et un élément ou attribut avec un type simple est mappé avec la colonne du même nom en tant qu'élément ou attribut.  
  
 Ces annotations peuvent également être utilisées pour spécifier les relations hiérarchiques en XML et représenter ainsi les relations dans la base de données, parce qu'un schéma XSD est simplement une vue XML de données relationnelles.  
  
 Cette section fournit les descriptions des annotations que vous pouvez utiliser avec les schémas XSD, ainsi que des exemples d'utilisation.  
  
> [!NOTE]  
>  Tous les exemples de cette section spécifient des requêtes XPath simples sur le schéma XSD annoté décrit dans chaque exemple. Ces exemples présument que vous connaissiez le langage XPath.  
  
## <a name="in-this-section"></a>Dans cette section  
 [Annotations XSD &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/xsd-annotations-sqlxml-4-0.md)  
 Répertorie les annotations que vous pouvez utiliser avec les schémas XSD, leurs descriptions et les annotations équivalentes pour XDR.  
  
 [Mappage par défaut des éléments XSD et des attributs aux Tables et colonnes &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/default-mapping-of-xsd-elements-and-attributes-to-tables-and-columns-sqlxml-4-0.md)  
 Explique le mappage par défaut et fournit des exemples de tâches en rapport avec le mappage par défaut.  
  
 [Mappage explicite d’éléments XSD et d’attributs aux Tables et colonnes &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/explicit-mapping-xsd-elements-and-attributes-to-tables-and-columns.md)  
 Explique le mappage explicite avec le **SQL : relation** et **SQL : Field** annotations et fournit des exemples.  
  
 [Spécification de relations à l’aide de SQL : Relationship &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/specifying-relationships-using-sql-relationship-sqlxml-4-0.md)  
 Décrit et fournit des exemples de la **SQL : Relationship** annotation.  
  
 [Spécification de l’attribut SQL : inverse sur SQL : Relationship &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/specifying-the-sql-inverse-attribute-on-sql-relationship-sqlxml-4-0.md)  
 Décrit la **SQL : inverse** annotation.  
  
 [Création d’éléments constants à l’aide de sql : constante est &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/creating-constant-elements-using-sql-is-constant-sqlxml-4-0.md)  
 Décrit et fournit des exemples de la **sql : constante est** annotation.  
  
 [Exclusion d’éléments de schéma du Document XML résultant avec sql : mappé &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/excluding-schema-elements-from-the-xml-document-using-sql-mapped.md)  
 Décrit et fournit des exemples de la **sql : mappé** annotation.  
  
 [Filtrage des valeurs à l’aide de SQL : limit-champ et SQL : limit-valeur &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/filtering-values-using-sql-limit-field-and-sql-limit-value-sqlxml-4-0.md)  
 Décrit et fournit des exemples de la **SQL : limit-champ** et **SQL : limit-valeur** annotations.  
  
 [Identification des colonnes de clés à l’aide de SQL : Key-champs &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/identifying-key-columns-using-sql-key-fields-sqlxml-4-0.md)  
 Décrit et fournit des exemples de la **SQL : Key-champs** annotation.  
  
 [Spécification d’une cible Namespace à l’aide de l’attribut targetNamespace &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/specifying-a-target-namespace-using-the-targetnamespace-attribute-sqlxml-4-0.md)  
 Décrit et fournit des exemples de la **targetNamespace** attribut.  
  
 [Création d’ID valide, IDREF et IDREFS à des attributs de Type à l’aide de SQL : Prefix &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/creating-valid-id-idref-and-idrefs-type-attributes-using-sql-prefix-sqlxml-4-0.md)  
 Décrit et fournit des exemples de la **SQL : Prefix** annotation.  
  
 [Conversions de types de données et de l’Annotation SQL : DataType &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/data-type-coercions-and-the-sql-datatype-annotation-sqlxml-4-0.md)  
 Décrit et fournit des exemples de la **SQL : DataType** annotation.  
  
 [Mappage des Types de données XSD aux Types de données XPath &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/mapping-xsd-data-types-to-xpath-data-types-sqlxml-4-0.md)  
 Fournit une table qui compare les types de données XSD, XDR et Xpath, et répertorie les conversions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] appropriées.  
  
 [Création à l’aide des Sections CDATA de SQL : use-cdata &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/creating-cdata-sections-using-sql-use-cdata-sqlxml-4-0.md)  
 Décrit et fournit des exemples de la **SQL : Use-données** annotation.  
  
 [Demande de références URL à des données d’objet BLOB à l’aide de sql : encode &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/requesting-url-references-to-blob-data-using-sql-encode-sqlxml-4-0.md)  
 Décrit et fournit des exemples de la **sql : encode** annotation.  
  
 [La récupération des données à l’aide de SQL : Overflow-champ &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-using/retrieving-unconsumed-data-using-the-sql-overflow-field-sqlxml-4-0.md)  
 Décrit et fournit des exemples de la **SQL : Overflow-champ** annotation.  
  
 [Masquage d’éléments et d’attributs à l’aide de sql:hide](../../relational-databases/sqlxml-annotated-xsd-schemas-using/hiding-elements-and-attributes-by-using-sql-hide.md)  
 Décrit et fournit des exemples de la **SQL : Hide** annotation.  
  
 [Utilisation des annotations sql:identity et sql:guid](../../relational-databases/sqlxml-annotated-xsd-schemas-using/using-the-sql-identity-and-sql-guid-annotations.md)  
 Décrit et fournit des exemples de la **: Identity** et **SQL : GUID** annotations.  
  
 [Spécification de la profondeur dans les relations récursives à l’aide de sql:max-depth](../../relational-databases/sqlxml-annotated-xsd-schemas-using/specifying-depth-in-recursive-relationships-by-using-sql-max-depth.md)  
 Décrit et fournit des exemples de la **SQL : max-depth** annotation.  
  
## <a name="see-also"></a>Voir aussi  
 [Annoté considérations de sécurité de schéma &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/security/annotated-schema-security-considerations-sqlxml-4-0.md)  
  
  
