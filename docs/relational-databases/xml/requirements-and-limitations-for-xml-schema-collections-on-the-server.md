---
title: Spécifications et limitations relatives aux collections de schémas XML sur le serveur | Microsoft Docs
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
- identifiers [XML schema collections]
- XML schema collections [SQL Server], limitations
- substitution groups [XML in SQL Server]
- XML schema collections [SQL Server], guidelines
- lax validation
- enumeration facets [XML in SQL Server]
- decimal precision [XML in SQL Server]
- repeated XML schema collection values
- schema collections [SQL Server], limitations
- time zones [XML in SQL Server]
- precision decimals [XML in SQL Server]
- schema collections [SQL Server], guidelines
- lexical representation
ms.assetid: c2314fd5-4c6d-40cb-a128-07e532b40946
caps.latest.revision: 84
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: cfece11f3cec121dcdd56a3918f3bdafc7bbd05e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="requirements-and-limitations-for-xml-schema-collections-on-the-server"></a>Spécifications et limitations relatives aux collections de schémas XML sur le serveur
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]
  La validation XSD (XML Schema Definition Language) des colonnes SQL utilisant le type de données **xml** est soumise à certaines limitations, lesquelles sont exposées dans le tableau suivant. Les recommandations à suivre pour modifier un schéma XSD afin qu'il soit compatible avec [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]y sont abordées. Les rubriques de cette section fournissent des informations supplémentaires concernant les limitations spécifiques, ainsi qu'une assistance pour leur utilisation.  
  
|Élément|Limitation|  
|----------|----------------|  
|**minOccurs** et **maxOccurs**|Les valeurs des attributs **minOccurs** et **maxOccurs** doivent tenir dans des entiers à 4 octets. Les schémas non conformes sont rejetés par le serveur.|  
|**\<xsd:choice>**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] refuse les schémas comportant une particule **\<xsd:choice>** sans enfant, sauf si cette particule est définie avec une valeur d’attribut **minOccurs** de zéro.|  
|**\<xsd:include>**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne prend actuellement pas en charge cet élément. Les schémas XML incluant cet élément seront rejetés par le serveur.<br /><br /> En guise de solution, les schémas XML qui comportent la directive **\<xsd:include>** peuvent être prétraités de façon à copier et fusionner le contenu de tous les schémas inclus dans un seul et unique schéma à charger vers le serveur. Pour plus d’informations, consultez [Prétraiter un schéma pour fusionner des schémas inclus](../../relational-databases/xml/preprocess-a-schema-to-merge-included-schemas.md).|  
|**\<xsd:key>**, **\<xsd:keyref>** et **\<xsd:unique>**|Actuellement, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne prend pas en charge ces contraintes basées sur XSD pour appliquer l'unicité ou établir des clés ou des références de clés. Les schémas XML contenant ces éléments ne peuvent pas être inscrits.|  
|**\<xsd:redefine>**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne prend actuellement pas en charge cet élément. Pour obtenir des informations sur une autre façon de mettre à jour des schémas, consultez [Élément &#60;xsd:redefine&#62;](../../relational-databases/xml/the-xsd-redefine-element.md)y sont abordées.|  
|**Valeurs \<xsd:simpleType>**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend uniquement en charge la précision à la milliseconde pour les types simples comportant un composant seconde autre que **xs:time** et **xs:dateTime**, et une précision de 100 nanosecondes pour **xs:time** et **xs:dateTime**. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] impose des limites à toutes les énumérations de types simples XSD reconnus.<br /><br /> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne prend pas en charge l’utilisation de la valeur « NaN » dans les déclarations **\<xsd:simpleType>**.<br /><br /> Pour plus d’informations, consultez[Valeurs pour les déclarations &#60;xsd:simpleType&#62;](../../relational-databases/xml/values-for-xsd-simpletype-declarations.md)y sont abordées.|  
|**xsi:schemaLocation** et **xsi:noNamespaceSchemaLocation**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ignore ces attributs s’ils sont présents dans les données d’instance XML insérées dans une colonne ou une variable dont le type de données est **xml** .|  
|**xs:QName**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne prend pas en charge les types dérivés de **xs:QName** utilisant un élément de restriction de schéma XML.<br /><br /> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne prend pas en charge les types union avec **xs:QName** en tant qu'élément de membre.<br /><br /> Pour plus d’informations, consultez [The xs:QName Type](../../relational-databases/xml/the-xs-qname-type.md).|  
|Ajout de membres à un groupe de substitution existant|Vous ne pouvez pas ajouter des membres à un groupe de substitution existant dans une collection de schémas XML. Un groupe de substitution dans un schéma XML est restreint en cela que l’élément de tête et tous les éléments membres doivent être définis dans la même instruction {CREATE &#124; ALTER} XML SCHEMA COLLECTION.|  
|Formes canoniques et restrictions de modèle|La représentation canonique d'une valeur ne peut enfreindre la restriction de modèle pour son type. Pour plus d’informations, consultez [Canonical Forms and Pattern Restrictions](../../relational-databases/xml/canonical-forms-and-pattern-restrictions.md).|  
|Facettes d'énumération|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne prend pas en charge les schémas XML avec des types présentant des facettes de modèles ou des énumérations enfreignant ces facettes.|  
|Longueur de facette|Les facettes **length**, **minLength**et **maxLength** sont stockées sous la forme d'un type **long** , type codé sur 32 bits. Par conséquent, la plage de valeurs acceptables pour ces valeurs est 2^31.|  
|Attribut d'ID|Chaque composant de schéma XML peut avoir un attribut d'ID. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] applique l’unicité des déclarations **\<xsd:attribute>** de type **ID**, mais il ne stocke pas ces valeurs. L’étendue pour l’application de l’unicité est l’instruction {CREATE &#124; ALTER} XML SCHEMA COLLECTION.|  
|Type ID|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne prend pas en charge les éléments de type **xs:ID**, **xs:IDREF**ou **xs:IDREFS**. Un schéma peut ne pas déclarer les éléments de ce type, ou les éléments dérivés par restriction ou par extension de ce type.|  
|Espace de noms local|L’espace de noms local doit être spécifié explicitement pour l’élément **\<xsd:any>**. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] rejette les schémas qui utilisent une chaîne vide ("") comme valeur de l’attribut d’espace de noms. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nécessite en lieu et place l'utilisation explicite de « ##local » pour indiquer qu'un élément ou un attribut non qualifié sera utilisé en tant qu'instance du caractère générique.|  
|Type mixte et contenu simple|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne prend pas en charge la restriction d'un type mixte à du contenu simple. Pour plus d’informations, consultez [Mixed Type and Simple Content](../../relational-databases/xml/mixed-type-and-simple-content.md).|  
|Type NOTATION|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n'accepte pas les valeurs de type NOTATION.|  
|Conditions de mémoire insuffisante|La manipulation de collections de schémas XML volumineuses peut entraîner des conditions de mémoire insuffisante. Pour trouver des solutions à ces problèmes, consultez [Collections de schémas XML volumineuses et conditions de mémoire insuffisante](../../relational-databases/xml/large-xml-schema-collections-and-out-of-memory-conditions.md).|  
|Valeurs répétées|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] rejette les schémas où l'attribut block ou final comporte des valeurs répétées comme « restriction restriction » et « extension extension ».|  
|Identificateurs de composant de schéma|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] limite les identificateurs de composant de schéma à une longueur maximale de 1 000 caractères Unicode. De plus, l'emploi de paires de caractères de substitution au sein des identificateurs n'est pas pris en charge.|  
|Informations de fuseau horaire|Dans [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et les versions ultérieures, les informations de fuseau horaire sont totalement prises en charge pour les valeurs **xs:date**, **xs:time**et **xs:dateTime** pour la validation de schéma XML. Avec le mode de compatibilité descendante de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] , les informations de fuseau horaire sont toujours normalisées en Temps universel coordonné (heure de Greenwich). Pour les éléments de type **dateTime** , le serveur convertit l’heure fournie en heure GMT en se basant sur la valeur de décalage horaire (« -05:00 ») et en retournant l’heure GMT correspondante.|  
|Types union|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne prend pas en charge les restrictions des types union.|  
|Décimales de précision de variable|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne prend pas en charge les décimales de précision de variable. Le type **xs:decimal** représente des nombres de décimales de précision arbitraire. Les processeurs XML conformes minimaux doivent prendre en charge les nombres décimaux avec un minimum de `totalDigits=18`. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend en charge `totalDigits=38,` , mais limite les chiffres fractionnels à 10. Toutes les valeurs instanciées de **xs:decimal** sont représentées en interne par le serveur en utilisant le type numeric SQL (38, 10).|  
  
## <a name="in-this-section"></a>Dans cette section  
  
|Rubrique|Description|  
|-----------|-----------------|  
|[Canonical Forms and Pattern Restrictions](../../relational-databases/xml/canonical-forms-and-pattern-restrictions.md)|Explique les formes canoniques et restrictions de modèle.|  
|[Composants génériques et validation de contenu](../../relational-databases/xml/wildcard-components-and-content-validation.md)|Décrit les limitations liées à l'utilisation des caractères génériques, de la validation de type lax et des éléments anyType avec des collections de schémas XML.|  
|[Élément &#60;xsd:redefine&#62;](../../relational-databases/xml/the-xsd-redefine-element.md)|Explique la limitation liée à l’utilisation de l’élément \<xsd:redefine> et décrit une solution de contournement.|  
|[Type xs:QName](../../relational-databases/xml/the-xs-qname-type.md)|Décrit la limitation relative au type xs:QName.|  
|[Valeurs pour les déclarations &#60;xsd:simpleType&#62;](../../relational-databases/xml/values-for-xsd-simpletype-declarations.md)|Décrit les restrictions appliquées aux déclarations \<xsd:simpleType>.|  
|[Facettes d’énumération](../../relational-databases/xml/enumeration-facets.md)|Décrit la limitation relative aux facettes d'énumération.|  
|[Mixed Type and Simple Content](../../relational-databases/xml/mixed-type-and-simple-content.md)|Décrit la limitation relative à la restriction d'un type mixte à un contenu simple.|  
|[Collections de schémas XML volumineuses et conditions de mémoire insuffisante](../../relational-databases/xml/large-xml-schema-collections-and-out-of-memory-conditions.md)|Fournit des solutions pour la condition de mémoire insuffisante qui se produit parfois avec les grandes collections de schémas.|  
|[Modèles de contenu non déterministes](../../relational-databases/xml/non-deterministic-content-models.md)|Décrit les limitations relatives aux modèles de contenu non déterministes.|  
  
## <a name="see-also"></a> Voir aussi  
 [Données XML &#40;SQL Server&#41;](../../relational-databases/xml/xml-data-sql-server.md)   
 [Comparer du XML typé et du XML non typé](../../relational-databases/xml/compare-typed-xml-to-untyped-xml.md)   
 [Accorder des autorisations sur une collection de schémas XML](../../relational-databases/xml/grant-permissions-on-an-xml-schema-collection.md)   
 [Contrainte d'attribution de particule unique](../../relational-databases/xml/unique-particle-attribution-constraint.md)   
 [Collections de schémas XML &#40;SQL Server&#41;](../../relational-databases/xml/xml-schema-collections-sql-server.md)  
  
  
