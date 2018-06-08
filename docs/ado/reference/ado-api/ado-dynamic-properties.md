---
title: Propriétés dynamiques ADO | Documents Microsoft
ms.prod: sql
ms.prod_service: drivers
ms.service: ''
ms.component: reference
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.suite: sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- dynamic properties [ADO]
- properties [ADO], dynamic
ms.assetid: d7b06d72-f792-4328-93a2-5006b9e2c581
caps.latest.revision: 12
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 93d699a75045afde3b03a9f977657d0d6db87933
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="ado-dynamic-properties"></a>Propriétés dynamiques ADO
Les propriétés dynamiques peuvent être ajoutées à la [propriétés](../../../ado/reference/ado-api/properties-collection-ado.md) collections de la [connexion](../../../ado/reference/ado-api/connection-object-ado.md), [commande](../../../ado/reference/ado-api/command-object-ado.md), ou [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) objets. La source de ces propriétés est un fournisseur de données, telles que la [fournisseur OLE DB pour SQL Server](../../../ado/guide/appendixes/microsoft-ole-db-provider-for-sql-server.md), ou un fournisseur de services, tels que les [le Service de curseur Microsoft pour OLE DB](../../../ado/guide/appendixes/microsoft-cursor-service-for-ole-db-ado-service-component.md). Consultez le fournisseur de données approprié ou la documentation pour plus d’informations sur une propriété dynamique spécifique du fournisseur de service.  
  
 Le [Index des propriétés dynamiques ADO](../../../ado/reference/ado-api/ado-dynamic-property-index.md) fournit une référence croisée entre les noms ADO et OLE DB pour chaque propriété dynamique standard de fournisseur OLE DB.  
  
 Les propriétés dynamiques suivantes sont particulièrement intéressantes et sont également documentées dans les sources qui ont été décrites précédemment. Les fonctionnalités spéciales avec ADO sont documentée dans les rubriques d’aide ADO dans la liste suivante.  
  
|||  
|-|-|  
|[Optimiser](../../../ado/reference/ado-api/optimize-property-dynamic-ado.md)|Spécifie si un index doit être créé sur ce champ.|  
|[invite de commandes](../../../ado/reference/ado-api/prompt-property-dynamic-ado.md)|Spécifie si le fournisseur OLE DB doit inviter l’utilisateur pour les informations d’initialisation.|  
|[Modifier la forme nom](../../../ado/reference/ado-api/reshape-name-property-dynamic-ado.md)|Spécifie un nom pour le **Recordset** objet.|  
|[Resynchronisation des commandes](../../../ado/reference/ado-api/resync-command-property-dynamic-ado.md)|Spécifie une commande fournie par l’utilisateur de chaîne qui le **Resync** les problèmes de méthode pour actualiser les données dans la table nommée dans la **Unique Table** propriété dynamique.|  
|[Table unique, schéma Unique, catalogue Unique](../../../ado/reference/ado-api/unique-table-unique-schema-unique-catalog-properties-dynamic-ado.md)|**Table unique** Spécifie le nom de la table de base sur laquelle les mises à jour, insertions et suppressions sont autorisées.<br /><br /> **Schéma unique** Spécifie le nom du propriétaire de la table ou le schéma.<br /><br /> **Catalogue unique** Spécifie le nom de la base de données qui contient la table ou le catalogue.|  
|[Resynchronisation de la mise à jour](../../../ado/reference/ado-api/update-resync-property-dynamic-ado.md)|Spécifie si le **UpdateBatch** méthode est suivie par implicite **Resync** opération de la méthode et dans ce cas, la portée de cette opération.|  
  
## <a name="see-also"></a>Voir aussi  
 [Référence des API ADO](../../../ado/reference/ado-api/ado-api-reference.md)   
 [Collections ADO](../../../ado/reference/ado-api/ado-collections.md)   
 [Constantes énumérées ADO](../../../ado/reference/ado-api/ado-enumerated-constants.md)   
 [Annexe b : erreurs ADO](../../../ado/guide/appendixes/appendix-b-ado-errors.md)   
 [Événements ADO](../../../ado/reference/ado-api/ado-events.md)   
 [Méthodes ADO](../../../ado/reference/ado-api/ado-methods.md)   
 [Modèle objet ADO](../../../ado/reference/ado-api/ado-object-model.md)   
 [Objets et interfaces ADO](../../../ado/reference/ado-api/ado-objects-and-interfaces.md)   
 [Propriétés ADO](../../../ado/reference/ado-api/ado-properties.md)
