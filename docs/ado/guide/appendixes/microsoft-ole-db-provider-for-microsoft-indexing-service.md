---
title: Fournisseur Microsoft OLE DB pour le Service d’indexation Microsoft | Documents Microsoft
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
- Indexing Service provider [ADO]
- providers [ADO], OLE DB provider for Microsoft Indexing service
- OLE DB provider for Microsoft Indexing service [ADO]
ms.assetid: f86a0598-5097-471b-8318-d2c859d085f2
caps.latest.revision: 15
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 8dc341794d419559b3683c06d51766d2e7b6287a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="microsoft-ole-db-provider-for-microsoft-indexing-service-overview"></a>Fournisseur Microsoft OLE DB pour l’indexation de vue d’ensemble du Service Microsoft
Le fournisseur Microsoft OLE DB pour le Service d’indexation Microsoft fournit un accès en lecture seule par programmation pour le système de fichiers et données Web indexées par le Service d’indexation Microsoft. Les applications ADO peuvent émettre des requêtes SQL pour récupérer des informations de propriété de contenu et de fichiers.

 Le fournisseur est libre de threads et UNICODE.

## <a name="connection-string-parameters"></a>Paramètres de chaîne de connexion
 Pour vous connecter à ce fournisseur, définissez la **fournisseur =** l’argument de la [ConnectionString](../../../ado/reference/ado-api/connectionstring-property-ado.md) propriété :

```
MSIDXS
```

 La lecture de la [fournisseur](../../../ado/reference/ado-api/provider-property-ado.md) propriété retournera également cette chaîne.

## <a name="typical-connection-string"></a>Chaîne de connexion classique
 Une chaîne de connexion par défaut pour ce fournisseur est :

```
"Provider=MSIDXS;Data Source=myCatalog;Locale Identifier=nnnn;"
```

 La chaîne se compose des mots clés suivants :

|Mot clé| Description|
|-------------|-----------------|
|**Fournisseur**|Spécifie le fournisseur OLE DB pour le Service d’indexation Microsoft. En général, ceci est le seul mot clé spécifié dans la chaîne de connexion.|
|**Source de données**|Spécifie le nom de catalogue du Service d’indexation. Si ce mot clé n’est pas spécifié, le catalogue système par défaut est utilisé.|
|**Identificateur de paramètres régionaux**|Spécifie un nombre 32 bits unique (par exemple, 1033) qui spécifie les préférences liées à la langue de l’utilisateur. Si ce mot clé n’est pas spécifié, l’identificateur de paramètres régionaux du système par défaut est utilisé.|

## <a name="command-text"></a>Texte de la commande
 La syntaxe de requête SQL du Service d’indexation se compose des extensions pour le SQL-92 **sélectionnez** instruction et son **FROM** et **où** clauses. Les résultats de la requête sont retournés via les ensembles de lignes OLE DB, qui peuvent être utilisés par ADO et manipulées en tant que [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) objets.

 Vous pouvez rechercher des mots exacts ou des expressions, ou utiliser des caractères génériques pour rechercher des modèles ou des racines de mots. La logique de recherche peut être basée sur des décisions booléennes, des termes pondérés ou proximité d’autres mots. Vous pouvez également rechercher en « texte libre », qui recherche les correspondances basée sur la signification, plutôt que les mots exacts.

 Le dialecte de commande spécifique est entièrement documenté dans les langages de requête pour la documentation du Service d’indexation.

 Le fournisseur n’accepte pas les appels de procédures stockées ou des noms de table simples (par exemple, le [CommandType](../../../ado/reference/ado-api/commandtype-property-ado.md) propriété sera toujours **adCmdText**).

## <a name="recordset-behavior"></a>Comportement du jeu d’enregistrements
 Les tableaux suivants répertorient les fonctionnalités disponibles avec un **Recordset** objet ouvert avec ce fournisseur. Seul le type de curseur statique (**adOpenStatic**) est disponible.

 Pour plus d’informations sur les **Recordset** comportement de configuration de votre fournisseur, exécutez le [prend en charge](../../../ado/reference/ado-api/supports-method.md) (méthode) et énumérer le [propriétés](../../../ado/reference/ado-api/properties-collection-ado.md) collection de la **Recordset** pour déterminer si les propriétés dynamiques spécifiques au fournisseur sont présentes.

 **Disponibilité des propriétés de jeu d’enregistrements ADO standard :**

|Propriété|Disponibilité|
|--------------|------------------|
|[AbsolutePage](../../../ado/reference/ado-api/absolutepage-property-ado.md)|lecture/écriture|
|[AbsolutePosition](../../../ado/reference/ado-api/absoluteposition-property-ado.md)|lecture/écriture|
|[ActiveConnection](../../../ado/reference/ado-api/activeconnection-property-ado.md)|Lecture seule|
|[BOF](../../../ado/reference/ado-api/bof-eof-properties-ado.md)|Lecture seule|
|[Signet](../../../ado/reference/ado-api/bookmark-property-ado.md)*|lecture/écriture|
|[CacheSize](../../../ado/reference/ado-api/cachesize-property-ado.md)|lecture/écriture|
|[CursorLocation](../../../ado/reference/ado-api/cursorlocation-property-ado.md)|toujours **adUseServer**|
|[CursorType](../../../ado/reference/ado-api/cursortype-property-ado.md)|toujours **adOpenStatic**|
|[EditMode](../../../ado/reference/ado-api/editmode-property.md)|toujours **adEditNone**|
|[EOF](../../../ado/reference/ado-api/bof-eof-properties-ado.md)|Lecture seule|
|[Filtre](../../../ado/reference/ado-api/filter-property.md)|lecture/écriture|
|[LockType](../../../ado/reference/ado-api/locktype-property-ado.md)|lecture/écriture|
|[MarshalOptions](../../../ado/reference/ado-api/marshaloptions-property-ado.md)|non disponible|
|[MaxRecords](../../../ado/reference/ado-api/maxrecords-property-ado.md)|lecture/écriture|
|[PageCount](../../../ado/reference/ado-api/pagecount-property-ado.md)|Lecture seule|
|[PageSize](../../../ado/reference/ado-api/pagesize-property-ado.md)|lecture/écriture|
|[RecordCount](../../../ado/reference/ado-api/recordcount-property-ado.md)|Lecture seule|
|[Source](../../../ado/reference/ado-api/source-property-ado-recordset.md)|lecture/écriture|
|[État](../../../ado/reference/ado-api/state-property-ado.md)|Lecture seule|
|[État](../../../ado/reference/ado-api/status-property-ado-recordset.md)|Lecture seule|

 \*Signets doivent être activés sur le fournisseur afin que cette fonctionnalité existe sur le **Recordset**.

 **Disponibilité des méthodes de jeu d’enregistrements ADO standard :**

|Méthode|Disponible ?|
|------------|----------------|
|[AddNew](../../../ado/reference/ado-api/addnew-method-ado.md)|non|
|[Annuler](../../../ado/reference/ado-api/cancel-method-ado.md)|Oui|
|[CancelBatch](../../../ado/reference/ado-api/cancelbatch-method-ado.md)|non|
|[CancelUpdate](../../../ado/reference/ado-api/cancelupdate-method-ado.md)|non|
|[Clone](../../../ado/reference/ado-api/clone-method-ado.md)|Oui|
|[Fermer](../../../ado/reference/ado-api/close-method-ado.md)|Oui|
|[Delete](../../../ado/reference/ado-api/delete-method-ado-recordset.md)|non|
|[GetRows](../../../ado/reference/ado-api/getrows-method-ado.md)|Oui|
|[Déplacer](../../../ado/reference/ado-api/move-method-ado.md)|Oui|
|[MoveFirst](../../../ado/reference/ado-api/movefirst-movelast-movenext-and-moveprevious-methods-ado.md)|Oui|
|[NextRecordset](../../../ado/reference/ado-api/nextrecordset-method-ado.md)|Oui|
|[Ouvrir](../../../ado/reference/ado-api/open-method-ado-recordset.md)|Oui|
|[Requery)](../../../ado/reference/ado-api/requery-method.md)|Oui|
|[Resynchronisation](../../../ado/reference/ado-api/resync-method.md)|Oui|
|[Prise en charge](../../../ado/reference/ado-api/supports-method.md)|Oui|
|[Update](../../../ado/reference/ado-api/update-method.md)|non|
|[UpdateBatch](../../../ado/reference/ado-api/updatebatch-method.md)|non|

 Pour les détails d’implémentation spécifiques et des informations fonctionnelles sur le fournisseur Microsoft OLE DB pour le Service d’indexation Microsoft, consultez le [Guide du programmeur OLE DB](https://msdn.microsoft.com/library/windows/desktop/ms713643.aspx), ou consultez la page des Services Web du serveur de site Web Windows NT site.

## <a name="see-also"></a>Voir aussi
 [CommandType, propriété (ADO)](../../../ado/reference/ado-api/commandtype-property-ado.md) [ConnectionString, propriété (ADO)](../../../ado/reference/ado-api/connectionstring-property-ado.md) [Collection de propriétés (ADO)](../../../ado/reference/ado-api/properties-collection-ado.md) [fournisseur, propriété (ADO)](../../../ado/reference/ado-api/provider-property-ado.md) [ L’objet Recordset (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md) [prend en charge (méthode)](../../../ado/reference/ado-api/supports-method.md)
