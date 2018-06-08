---
title: Enregistrer l’objet (ADO) | Documents Microsoft
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
- Record
helpviewer_keywords:
- Record object [ADO]
ms.assetid: db83ed2c-a8e3-460c-8682-64667e4d5d01
caps.latest.revision: 6
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 0bd0c6e6e5ad0e9d5ff059e0e9f4125137fb08fc
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="record-object-ado"></a>Objet d’enregistrement (ADO)
Représente une ligne d’un [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) ou le fournisseur de données, ou un objet retourné par un fournisseur de données semi-structurées, par exemple un fichier ou répertoire.  
  
## <a name="remarks"></a>Notes  
 A **enregistrement** objet représente une ligne de données et présente certaines ressemblances conceptuels avec une seule ligne **Recordset**. En fonction des capacités de votre fournisseur, **enregistrement** objets peuvent être retournés directement à partir de votre fournisseur au lieu d’une ligne **Recordset**, par exemple, lorsqu’une requête SQL qui sélectionne une seule ligne est exécutée. Ou, un **enregistrement** objet peut être obtenu directement à partir d’un **Recordset** objet. Ou, un **enregistrement** peuvent être retournées directement à partir d’un fournisseur de données semi-structurées, tel que le fournisseur OLE DB pour Microsoft Exchange.  
  
 Vous pouvez afficher les champs associés le **enregistrement** de l’objet par le biais de la [champs](../../../ado/reference/ado-api/fields-collection-ado.md) collection sur le **enregistrement** objet. Valeur d’objet de colonnes, y compris **Recordset**, **SafeArray**et les valeurs scalaires dans le **champs** collection de **enregistrement** objets.  
  
 Si le **enregistrement** objet représente une ligne dans un **Recordset**, il est possible de revenir à ce d’origine **Recordset** avec la [Source](../../../ado/reference/ado-api/source-property-ado-record.md) propriété.  
  
 Le **enregistrement** objet peut également être utilisé par les fournisseurs de données semi-structurées comme le [fournisseur Microsoft OLE DB pour Internet Publishing](../../../ado/guide/appendixes/microsoft-ole-db-provider-for-internet-publishing.md), à l’arborescence des espaces de noms de modèle. Chaque nœud dans l’arborescence est un **enregistrement** objet avec ses colonnes associées. Les colonnes peuvent représenter les attributs de ce nœud et d’autres informations pertinentes. Le **enregistrement** objet peut représenter un nœud feuille et un nœud non-feuille dans l’arborescence. Nœuds non terminaux possèdent d’autres nœuds en tant que leur contenu, mais les nœuds terminaux n’ont pas ce contenu. Les nœuds terminaux contiennent généralement des flux de données binaires et nœuds non terminaux peuvent également avoir un flux binaire par défaut qui s’y rapportent. Propriétés de la **enregistrement** objet identifier le type de nœud.  
  
 Le **enregistrement** objet représente également un autre moyen pour la navigation hiérarchique organisée de données. A **enregistrement** objet peut être créé pour représenter la racine d’une sous-arborescence spécifique dans une structure arborescente volumineux et nouveaux **enregistrement** objets pouvant être ouverts pour représenter des nœuds enfants.  
  
 Une ressource (par exemple, un fichier ou répertoire) peut être identifiée par une URL absolue. A [connexion](../../../ado/reference/ado-api/connection-object-ado.md) objet est implicitement créé et défini à la **enregistrement** objet lors de la **enregistrement** est ouvert à l’aide d’une URL absolue. A **connexion** objet explicitement peut être défini sur le **enregistrement** de l’objet le [ActiveConnection](../../../ado/reference/ado-api/activeconnection-property-ado.md) propriété. Les fichiers et répertoires qui sont accessibles à l’aide de la **connexion** objet définissent le *contexte* dans lequel **enregistrement** opérations peuvent se produire.  
  
 Méthodes de navigation et de la modification des données sur le **enregistrement** objet également accepter une URL relative, ce qui permet de trouver une ressource à l’aide d’une URL absolue ou **connexion** contexte de l’objet en tant que point de départ.  
  
> [!NOTE]
>  URL à l’aide du modèle http appellent automatiquement le [fournisseur Microsoft OLE DB pour la publication Internet](../../../ado/guide/appendixes/microsoft-ole-db-provider-for-internet-publishing.md). Pour plus d’informations, consultez [URL absolues et relatives](../../../ado/guide/data/absolute-and-relative-urls.md).  
  
 A **connexion** objet est associé à chaque **enregistrement** objet. Par conséquent, **enregistrement** opérations de l’objet peuvent faire partie d’une transaction en appelant **connexion** méthodes de transaction de l’objet.  
  
 Le **enregistrement** objet ne prend pas en charge les événements ADO et ne peut donc répondre aux notifications.  
  
 Les méthodes et les propriétés d’un **enregistrement** de l’objet, vous pouvez procédez comme suit :  
  
-   Définir ou retourner associé **connexion** de l’objet avec la [ActiveConnection](../../../ado/reference/ado-api/activeconnection-property-ado.md) propriété.  
  
-   Indiquer les autorisations d’accès avec le [Mode](../../../ado/reference/ado-api/mode-property-ado.md) propriété.  
  
-   Retourne l’URL du répertoire, cas échéant, qui contient la ressource représentée par le **enregistrement** avec la [ParentURL](../../../ado/reference/ado-api/parenturl-property-ado.md) propriété.  
  
-   Indiquer l’URL absolue, l’URL relative, ou **Recordset** à partir de laquelle le **enregistrement** est dérivée avec le [Source](../../../ado/reference/ado-api/source-property-ado-record.md) propriété.  
  
-   Indiquer l’état actuel de la **enregistrement** avec la [état](../../../ado/reference/ado-api/state-property-ado.md) propriété.  
  
-   Indiquer le type de **enregistrement** : *simple*, *collection*, ou *document structuré* : avec la [RecordType](../../../ado/reference/ado-api/recordtype-property-ado.md)propriété.  
  
-   Arrêter l’exécution d’une opération asynchrone avec le [Annuler](../../../ado/reference/ado-api/cancel-method-ado.md) (méthode).  
  
-   Dissocier le **enregistrement** à partir d’une source de données avec le [fermer](../../../ado/reference/ado-api/close-method-ado.md) (méthode).  
  
-   Copiez le fichier ou le répertoire représenté par un **enregistrement** vers un autre emplacement avec le [CopyRecord](../../../ado/reference/ado-api/copyrecord-method-ado.md) (méthode).  
  
-   Supprimez le fichier, ou de répertoire et de ses sous-répertoires, représenté par un **enregistrement** avec la [DeleteRecord](../../../ado/reference/ado-api/deleterecord-method-ado.md) (méthode).  
  
-   Ouvrir un **Recordset** qui contient des lignes qui représentent les sous-répertoires et les fichiers de l’entité représentée par le **enregistrement** avec la [GetChildren](../../../ado/reference/ado-api/getchildren-method-ado.md) (méthode).  
  
-   Déplacer (renommer) le fichier, ou de répertoire et de ses sous-répertoires, représenté par un **enregistrement** vers un autre emplacement avec le [MoveRecord](../../../ado/reference/ado-api/moverecord-method-ado.md) (méthode).  
  
-   Associer le **enregistrement** avec des données existantes de source, ou créer un nouveau fichier ou un répertoire avec le [ouvrir](../../../ado/reference/ado-api/open-method-ado-record.md) (méthode).  
  
 Le **enregistrement** objet est sécurisé pour le script.  
  
 Cette section contient les rubriques suivantes.  
  
-   [Propriétés, méthodes et événements de l’objet Record](../../../ado/reference/ado-api/record-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Collection de champs (ADO)](../../../ado/reference/ado-api/fields-collection-ado.md)   
 [Collection de propriétés (ADO)](../../../ado/reference/ado-api/properties-collection-ado.md)   
 [Enregistrements et flux](../../../ado/guide/data/records-and-streams.md)   
 [Recordset, objet (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md)
