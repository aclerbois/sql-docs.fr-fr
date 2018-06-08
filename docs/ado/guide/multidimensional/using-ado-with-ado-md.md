---
title: À l’aide d’ADO avec ADO MD | Documents Microsoft
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
- ADO, using with ADO MD
ms.assetid: cfae435e-2ac3-4312-8c1e-9ca4a74cd875
caps.latest.revision: 12
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 00a999350e2aed86e74bb0bfaea77c05207a21a6
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="using-ado-with-ado-md"></a>À l’aide d’ADO avec ADO MD
ADO et ADO MD sont des modèles d’objet apparentés mais distincts. ADO fournit des objets de connexion aux sources de données, l’exécution de commandes, la récupération de données tabulaires et métadonnées de schéma dans un format tabulaire et affichage des informations d’erreur de fournisseur. ADO MD fournit des objets pour la récupération de données multidimensionnelles et l’affichage des métadonnées du schéma multidimensionnel.  
  
 Lorsque vous travaillez avec un MDP, vous pouvez choisir d’utiliser ADO, ADO MD ou les deux avec votre application. En référençant les deux bibliothèques dans votre projet, avoir un accès complet aux fonctionnalités fournies par votre fournisseur MDP.  
  
 Il est souvent utile d’obtenir une vue tabulaire aplatie d’un dataset multidimensionnel. Vous pouvez pour cela utiliser ADO [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) objet. Spécifiez la source pour votre [ensemble de cellules](../../../ado/reference/ado-md-api/cellset-object-ado-md.md) en tant que le ***Source*** paramètre pour le [ouvrir](../../../ado/reference/ado-api/open-method-ado-recordset.md) méthode d’un **Recordset**, plutôt que comme source pour un ADO MD **ensemble de cellules**.  
  
 Il peut également être utile d’afficher les métadonnées de schéma dans une vue tabulaire plutôt que comme une hiérarchie d’objets. ADO [OpenSchema](../../../ado/reference/ado-api/openschema-method.md) méthode sur le [connexion](../../../ado/reference/ado-api/connection-object-ado.md) objet permet à l’utilisateur d’ouvrir un **Recordset** qui contient des informations de schéma. Le ***QueryType*** paramètre de la **OpenSchema** méthode dispose de plusieurs [SchemaEnum](../../../ado/reference/ado-api/schemaenum.md) les valeurs qui se rapportent aux fournisseurs MDP. Ces valeurs sont les suivantes :  
  
-   **adSchemaCubes**  
  
-   **adSchemaDimensions**  
  
-   **adSchemaHierarchies**  
  
-   **adSchemaLevels**  
  
-   **adSchemaMeasures**  
  
-   **adSchemaMembers**  
  
 Pour utiliser des valeurs d’énumération ADO avec ADO MD propriétés ou méthodes, votre projet doit faire référence à la fois ADO et ADO MD bibliothèques. Par exemple, vous pouvez utiliser ADO **adState** des valeurs d’énumération avec la ADO MD [état](../../../ado/reference/ado-md-api/state-property-ado-md.md) propriété. Pour plus d’informations sur la façon de créer des références aux bibliothèques, consultez la documentation de votre outil de développement.  
  
 Pour plus d’informations sur les objets ADO et les méthodes, consultez le [référence des API ADO](../../../ado/reference/ado-api/ado-api-reference.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Modèle objet ADO MD](../../../ado/reference/ado-md-api/ado-md-object-model.md)   
 [ADO (multidimensionnel) (ADO MD)](../../../ado/guide/multidimensional/ado-multidimensional-ado-md.md)   
 [Vue d’ensemble des schémas et données multidimensionnels](../../../ado/guide/multidimensional/overview-of-multidimensional-schemas-and-data.md)   
 [Programmation avec ADO MD](../../../ado/guide/multidimensional/programming-with-ado-md.md)   
 [Utilisation de données multidimensionnelles](../../../ado/guide/multidimensional/working-with-multidimensional-data.md)
