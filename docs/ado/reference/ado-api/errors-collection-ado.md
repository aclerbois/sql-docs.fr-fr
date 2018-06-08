---
title: Collection d’erreurs (ADO) | Documents Microsoft
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
apitype: COM
f1_keywords:
- Connection15::Errors
- Connection15::get_Errors
- Connection15::GetErrors
helpviewer_keywords:
- Errors collection [ADO]
ms.assetid: 290819e1-7b39-4e1e-a93b-801257138b00
caps.latest.revision: 10
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: e7c259dda6c491c1992c19ed0ebe3decdc4a42d3
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="errors-collection-ado"></a>Collection d’erreurs (ADO)
Contient tous les [erreur](../../../ado/reference/ado-api/error-object.md) objets créés en réponse à une erreur liée au fournisseur.  
  
## <a name="remarks"></a>Notes  
 Toute opération impliquant des objets ADO peut générer une ou plusieurs erreurs de fournisseur. Lorsqu’une erreur survient, un ou plusieurs **erreur** objets peuvent être placés dans le **erreurs** collection de la [connexion](../../../ado/reference/ado-api/connection-object-ado.md) objet. Lorsqu’une autre opération ADO génère une erreur, le **erreurs** collection est désactivée et le nouvel ensemble de **erreur** objets peuvent être placés dans le **erreurs** collection.  
  
 Chaque **erreur** objet représente une erreur de fournisseur spécifique, pas une erreur ADO. Les erreurs ADO sont exposées au mécanisme de gestion des exceptions runtime. Par exemple, dans Microsoft Visual Basic, l’occurrence d’une erreur ADO spécifique déclenchera un [onError](../../../ado/reference/rds-api/onerror-event-rds.md) événement et s’affichent dans le **Err** objet.  
  
 Les opérations ADO qui ne génèrent une erreur n’ont aucun effet le **erreurs** collection. Utilisez le [effacer](../../../ado/reference/ado-api/clear-method-ado.md) méthode pour effacer manuellement les **erreurs** collection.  
  
 Le jeu de **erreur** des objets dans le **erreurs** collection décrit toutes les erreurs qui se sont produites en réponse à une seule instruction. L’énumération des erreurs spécifiques dans le **erreurs** collection permet de déterminer la cause et l’origine d’une erreur plus précisément et prendre les mesures appropriées pour récupérer vos routines de gestion des erreurs.  
  
 Certaines propriétés et méthodes retournent des avertissements qui apparaissent sous la forme **erreur** des objets dans le **erreurs** collection mais qui n’empêchent pas l’exécution d’un programme. Avant d’appeler le [Resync](../../../ado/reference/ado-api/resync-method.md), [UpdateBatch](../../../ado/reference/ado-api/updatebatch-method.md), ou [CancelBatch](../../../ado/reference/ado-api/cancelbatch-method-ado.md) méthodes sur un [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) objet, le [ouvrir](../../../ado/reference/ado-api/open-method-ado-connection.md) méthode sur un **connexion** de l’objet, ou définir le [filtre](../../../ado/reference/ado-api/filter-property.md) propriété sur un **Recordset** de l’objet, appelez le **effacer**méthode sur le **erreurs** collection. De cette façon, vous pouvez lire la [nombre](../../../ado/reference/ado-api/count-property-ado.md) propriété de la **erreurs** collection pour tester les avertissements retournés.  
  
> [!NOTE]
>  Consultez le **erreur** rubrique d’objet pour une explication plus détaillée de la façon dont une seule opération ADO peut générer plusieurs erreurs.  
  
 Cette section contient les rubriques suivantes.  
  
-   [Propriétés de Collection d’erreurs, méthodes et événements](../../../ado/reference/ado-api/errors-collection-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Objet Error](../../../ado/reference/ado-api/error-object.md)   
 [Annexe A : Fournisseurs](../../../ado/guide/appendixes/appendix-a-providers.md)
