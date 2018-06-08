---
title: Les Interfaces et les objets ADO | Documents Microsoft
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
- ADO, objects and interfaces
- objects [ADO]
ms.assetid: d0b7e254-c89f-4406-b846-a060ef038c30
caps.latest.revision: 14
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 047e1a7a0cafee0b562edc5b3ec47f9f4dd3989d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="ado-objects-and-interfaces"></a>Objets et interfaces ADO
Les relations entre ces objets sont représentées dans le [modèle objet ADO](../../../ado/reference/ado-api/ado-object-model.md).  
  
 Chaque objet peut être contenu dans sa collection correspondante. Par exemple, un [erreur](../../../ado/reference/ado-api/error-object.md) objet peut être contenu dans un [erreurs](../../../ado/reference/ado-api/errors-collection-ado.md) collection. Pour plus d’informations, consultez [Collections ADO](../../../ado/reference/ado-api/ado-collections.md) ou une rubrique de regroupement spécifique.  
  
|||  
|-|-|  
|[IADOCommandConstruction](https://msdn.microsoft.com/library/windows/desktop/aa965677.aspx)|Utilisé pour récupérer la commande OLEDB sous-jacente à partir d’un objet ADOCommand.|  
|[ADORecordConstruction](../../../ado/reference/ado-api/adorecordconstruction-interface.md)|Construit un ADO **enregistrement** objet OLE DB **ligne** objet dans une application C/C++.|  
|[ADORecordsetConstruction](../../../ado/reference/ado-api/adorecordsetconstruction-interface.md)|Construit un ADO **Recordset** objet OLE DB **ensemble de lignes** objet dans une application C/C++.|  
|[ADOStreamConstruction, interface](../../../ado/reference/ado-api/adostreamconstruction-interface.md)|Construit un ADO **flux** objet OLE DB **IStream** objet dans une application C/C++.|  
|[Command](../../../ado/reference/ado-api/command-object-ado.md)|Définit une commande spécifique que vous avez l’intention d’exécuter par rapport à une source de données.<br /><br /> Le **commande** l’objet n’est pas sécurisé pour le script.|  
|[Connexion](../../../ado/reference/ado-api/connection-object-ado.md)|Représente une connexion ouverte à une source de données.<br /><br /> Le **connexion** objet est sécurisé pour le script.|  
|[IDSOShapeExtensions, interface](../../../ado/reference/ado-api/idsoshapeextensions-interface.md)|Obtient l’objet sous-jacent de la Source de données OLE DB pour le fournisseur SHAPE.|  
|[Erreur](../../../ado/reference/ado-api/error-object.md)|Contient des détails sur les erreurs d’accès aux données qui se rapportent à une seule opération impliquant le fournisseur.<br /><br /> Le **erreur** objet n’est pas sécurisé pour le script.|  
|[Champ](../../../ado/reference/ado-api/field-object.md)|Représente une colonne de données avec un type de données commun.|  
|[Paramètre](../../../ado/reference/ado-api/parameter-object.md)|Représente un paramètre ou un argument associé à un **commande** objet basé sur une procédure stockée ou une requête paramétrable.<br /><br /> Le **paramètre** objet n’est pas sécurisé pour le script.|  
|[Propriété](../../../ado/reference/ado-api/property-object-ado.md)|Représente une caractéristique dynamique d’un objet ADO qui est défini par le fournisseur.|  
|[Enregistrement](../../../ado/reference/ado-api/record-object-ado.md)|Représente une ligne d’un **Recordset**, ou un répertoire ou un fichier dans un système de fichiers. Le **enregistrement** objet est sécurisé pour le script.|  
|[Recordset](../../../ado/reference/ado-api/recordset-object-ado.md)|Représente le jeu d’enregistrements à partir d’une table de base ou les résultats d’une commande exécutée. À tout moment, le **Recordset** objet fait référence à un seul enregistrement dans le jeu en tant que l’enregistrement actif.<br /><br /> Le **Recordset** objet est sécurisé pour le script.|  
|[Flux de données](../../../ado/reference/ado-api/stream-object-ado.md)|Représente un flux de données binaire.<br /><br /> Le **flux** objet est sécurisé pour le script.|  
  
## <a name="see-also"></a>Voir aussi  
 [Référence des API ADO](../../../ado/reference/ado-api/ado-api-reference.md)   
 [Collections ADO](../../../ado/reference/ado-api/ado-collections.md)   
 [Propriétés dynamiques ADO](../../../ado/reference/ado-api/ado-dynamic-properties.md)   
 [Constantes énumérées ADO](../../../ado/reference/ado-api/ado-enumerated-constants.md)   
 [Annexe b : erreurs ADO](../../../ado/guide/appendixes/appendix-b-ado-errors.md)   
 [Événements ADO](../../../ado/reference/ado-api/ado-events.md)   
 [Méthodes ADO](../../../ado/reference/ado-api/ado-methods.md)   
 [Modèle objet ADO](../../../ado/reference/ado-api/ado-object-model.md)   
 [Propriétés ADO](../../../ado/reference/ado-api/ado-properties.md)
