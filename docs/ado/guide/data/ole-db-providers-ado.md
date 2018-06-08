---
title: Fournisseurs OLE DB (ADO) | Documents Microsoft
ms.prod: sql
ms.prod_service: connectivity
ms.component: ado
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.suite: sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- OLE DB providers [ADO]
- ADO, OLE DB providers
ms.assetid: 6e0488c3-934d-4976-99dc-65c580dc7a3c
caps.latest.revision: 11
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: fa8bcda85b1b149da9dcc66bed92e044de800f66
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="ole-db-providers-ado"></a>Fournisseurs OLE DB (ADO)
OLE DB définit un ensemble d’interfaces COM qui fournissent aux applications un accès uniforme aux données stockées dans diverses sources d’informations. Cette approche permet à une source de données partager ses données via les interfaces qui prennent en charge les fonctionnalités SGBD appropriées pour la source de données. Par conception, l’architecture hautes performances d’OLE DB est basé sur son utilisation d’un modèle de services flexible, basée sur le composant. Au lieu d’avoir un nombre défini de couches intermédiaires entre l’application et les données, OLE DB requiert que le nombre de composants strictement nécessaires pour accomplir une tâche particulière.  
  
 Par exemple, supposons que l’utilisateur souhaite exécuter une requête. Examinez les scénarios suivants :  
  
-   Les données résident dans une base de données relationnelle pour laquelle il existe un pilote ODBC, mais aucun fournisseur OLE DB natif : l’application utilise ADO pour communiquer avec le fournisseur OLE DB pour ODBC, qui charge ensuite le pilote ODBC approprié. Le pilote transmet l’instruction SQL au SGBD, qui extrait les données.  
  
-   Les données résident dans Microsoft SQL Server pour laquelle il existe un fournisseur OLE DB natif : l’application utilise ADO pour communiquer directement avec le fournisseur OLE DB pour Microsoft SQL Server. Aucun intermédiaires ne sont nécessaires.  
  
-   Les données résident dans Microsoft Exchange Server, pour lequel il existe un fournisseur OLE DB, mais qui n’expose pas un moteur de traitement des requêtes SQL : l’application utilise ADO pour communiquer avec le fournisseur OLE DB pour Microsoft Exchange et appelle un processeur de requêtes OLE DB composant pour traiter la requête.  
  
-   Les données résident dans le système de fichiers NTFS de Microsoft sous la forme de documents : accès aux données à l’aide d’un fournisseur OLE DB natif via le Service d’indexation Microsoft, qui indexe le contenu et les propriétés des documents dans le système de fichiers pour permettre à du contenu efficace recherches.  
  
 Dans les exemples précédents, l’application peut interroger les données. Besoins de l’utilisateur sont remplies avec un nombre minimal de composants. Dans chaque cas, des composants supplémentaires sont utilisées uniquement si nécessaire, et seuls les composants requis sont appelés. Ce chargement de la demande de composants réutilisables et partageables contribue fortement à hautes performances lorsque OLE DB est utilisée.  
  
 Fournisseurs sont divisés en deux catégories : ceux qui fournissent des données et ceux qui fournissent des services. Un fournisseur de données possède ses propres données et les expose sous forme de tableau à votre application. Un fournisseur de services encapsule un service en produisant et en consommant des données, pour enrichir les fonctionnalités de vos applications ADO. Un fournisseur de services peut être également défini comme un composant de service, qui doit fonctionner conjointement avec d’autres fournisseurs de services ou les composants.  
  
 ADO fournit une cohérence interface de niveau supérieur pour les différents fournisseurs OLE DB.  
  
 Cette section contient les rubriques suivantes.  
  
-   [Fournisseurs de données](../../../ado/guide/data/data-providers.md)  
  
-   [Fournisseurs et composants de services](../../../ado/guide/data/service-providers-and-components.md)
