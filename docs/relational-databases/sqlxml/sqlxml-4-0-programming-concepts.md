---
title: SQLXML 4.0 Concepts de programmation | Documents Microsoft
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: sqlxml
ms.reviewer: ''
ms.suite: sql
ms.technology: xml
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- SQLXML, about SQLXML
- SQLXML
ms.assetid: 5a11cda2-b8a3-4453-848f-641afdaa7024
caps.latest.revision: 38
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 0071c9f56d118c3d80a92d556c29b813a25cdee1
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlxml-40-programming-concepts"></a>Concepts de la programmation SQLXML 4.0
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  SQLXML 3.0 a été proposé comme version Web pour fournir des fonctionnalités XML côté client supplémentaires et des améliorations aux fonctionnalités existantes, telles que les schémas XSD annotés, le chargement XML en masse, la prise en charge des services Web (SOAP) et les codes de mise à jour (updategram).  
  
 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] a introduit SQLXML 4.0, qui offre les mêmes fonctionnalités que SQLXML 3.0, ainsi que des mises à jour supplémentaires afin de gérer les nouvelles fonctionnalités introduites avec [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)].  
  
 Cette section fournit des informations sur SQLXML 4.0.  
  
 [SQLXML n’est pas installé dans SQL Server](../../relational-databases/sqlxml/sqlxml-is-not-installed-in-sql-server.md)  
 Décrit comment installer SQLXML 4.0.  
  
 [Nouveautés de SQLXML 4.0 SP1](../../relational-databases/sqlxml/what-s-new-in-sqlxml-4-0-sp1.md)  
 Décrit les mises à jour et les améliorations de SQLXML 4.0 et fournit des liens vers les rubriques appropriées de cette documentation.  
  
 [Utilisation d’ADO pour exécuter des requêtes SQLXML 4.0](../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md)  
 Décrit comment utiliser ADO pour les requêtes SQLXML. ADO joue un rôle plus important dans SQLXML 4.0 que dans les versions antérieures.  
  
 [Prise en charge du type de données XML dans SQLXML 4.0](../../relational-databases/sqlxml/xml-data-type-support-in-sqlxml-4-0.md)  
 Décrit la prise en charge du type de données xml ajouté dans SQLXML 4.0.  
  
 [Configuration requise pour l’exécution des exemples SQLXML](../../relational-databases/sqlxml/requirements-for-running-sqlxml-examples.md)  
 Décrit les spécifications permettant de créer des exemples fonctionnels à partir des exemples SQLXML fournis.  
  
 [Mise en forme côté client et côté serveur &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml/formatting/client-side-and-server-side-formatting-sqlxml-4-0.md)  
 Fournit des informations du type « à propos de » et des comparaisons sur la mise en forme côté client et côté serveur, y compris la commande FOR XML permettant de construire des documents XML.  
  
 [Schémas XSD annotés dans SQLXML 4.0](../../relational-databases/sqlxml/annotated-xsd-schemas/annotated-xsd-schemas-in-sqlxml-4-0.md)  
 Fournit des informations sur l'utilisation des schémas XSD annotés dans SQLXML 4.0. Fournit aussi des informations sur les schémas XDR annotés pour une utilisation dans les applications héritées.  
  
 [Utilisation des requêtes XPath dans SQLXML 4.0](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/using-xpath-queries-in-sqlxml-4-0.md)  
 Décrit comment utiliser un sous-ensemble du langage XPath pour interroger les vues XML créées par un schéma XSD annoté, et fournit des exemples.  
  
 [Pour modifier les données dans SQLXML 4.0 à l’aide de codes](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/updategrams/using-updategrams-to-modify-data-in-sqlxml-4-0.md)  
 Fournit des informations sur les codes de mise à jour (updategram), qui modifient les données d'une base de données en travaillant sur les vues XML fournies par les schémas XSD (ou XDR) annotés.  
  
 [Effectuer le chargement en masse des données XML & #40 ; SQLXML 4.0 & #41 ;](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/bulk-load-xml/performing-bulk-load-of-xml-data-sqlxml-4-0.md)  
 Décrit comment charger en masse XML dans SQLXML 4.0.  
  
 [Composants d’accès aux données 4.0 SQLXML](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/data-access-components-provider/sqlxml-4-0-data-access-components-sqlxmloledb-provider.md)  
 Documente le fournisseur SQLXMLOLEDB et fournit des liens vers d'autre composants d'accès aux données SQLXML 4.0.  
  
 [Prise en charge SQLXML 4.0 pour Microsoft .NET Framework](http://msdn.microsoft.com/library/c18cf801-f893-4fbc-8e2b-c563f6108acf)  
 Décrit la prise en charge de SQLXML 4.0 pour le .NET Framework.  
  
 [Mise en cache des modèles, XSL et schémas &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/caching-templates-xml-schemas/caching-templates-xsl-and-schemas-sqlxml-4-0.md)  
 Décrit les fonctionnalités de mise en cache fournies dans SQLXML pour améliorer les performances.  
  
 [Considérations relatives à la sécurité SQLXML 4.0](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/security/sqlxml-4-0-security-considerations.md)  
 Traite les problèmes de sécurité relatifs à SQLXML 4.0.  
  
 [Recommandations et limitations de SQLXML 4.0](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/guidelines-and-limitations-of-sqlxml-4-0.md)  
 Répertorie les problèmes à se rappeler lors de l'utilisation de SQLXML 4.0.  
  
## <a name="see-also"></a>Voir aussi  
 [Données XML &#40;SQL Server&#41;](../../relational-databases/xml/xml-data-sql-server.md)  
  
  
