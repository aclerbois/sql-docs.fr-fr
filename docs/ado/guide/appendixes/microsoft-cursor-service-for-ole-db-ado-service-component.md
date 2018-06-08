---
title: Service de curseur Microsoft pour OLE DB (composant du Service ADO) | Documents Microsoft
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
- providers [ADO], cursor service for OLE DB
- cursor service for OLE DB [ADO]
ms.assetid: 420d0989-7cfb-4c66-a7b5-f4199d13165d
caps.latest.revision: 16
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: a829fa8510054489bdc8f310941d9526f25b82a9
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="microsoft-cursor-service-for-ole-db-overview"></a>Service de curseur Microsoft pour une vue d’ensemble de la base de données OLE
Le Service de curseur Microsoft pour OLE DB complète les fonctions de prise en charge de curseur des fournisseurs de données. Par conséquent, l’utilisateur perçoit relativement uniforme des fonctionnalités à partir de tous les fournisseurs de données.

 Le Service de curseur rend disponibles les propriétés dynamiques et améliore le comportement de certaines méthodes. Par exemple, le [optimiser](../../../ado/reference/ado-api/optimize-property-dynamic-ado.md) propriété dynamique permet la création d’index temporaires qui facilitent certaines opérations, telles que la [trouver](../../../ado/reference/ado-api/find-method-ado.md) (méthode).

 Le Service de curseur permet la prise en charge de la mise à jour par lot dans tous les cas. Il simule également des types de curseurs plus performante, tels que les curseurs dynamiques, lorsqu’un fournisseur de données peut fournir uniquement les curseurs d’une capacité inférieure, tels que les curseurs statiques.

## <a name="keyword"></a>Mot clé
 Pour appeler ce composant de service, définissez la [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) ou [connexion](../../../ado/reference/ado-api/connection-object-ado.md) l’objet [CursorLocation](../../../ado/reference/ado-api/cursorlocation-property-ado.md) propriété **adUseClient**.

```
connection.CursorLocation=adUseClient
recordset.CursorLocation=adUseClient
```

## <a name="dynamic-properties"></a>Propriétés dynamiques
 Lorsque le Service de curseur pour OLE DB est appelé, les propriétés dynamiques suivantes sont ajoutées à la **Recordset** l’objet [propriétés](../../../ado/reference/ado-api/properties-collection-ado.md) collection. La liste complète des **connexion** et **Recordset** propriétés dynamiques de l’objet est répertorié dans le [Index des propriétés dynamiques ADO](../../../ado/reference/ado-api/ado-dynamic-property-index.md). Les noms de propriété OLE DB associés, le cas échéant, sont inclus dans les parenthèses après le nom de la propriété ADO.

 Modifications apportées à certaines propriétés dynamiques ne sont pas visibles à la source de données sous-jacente, une fois que le Service de curseur a été appelé. Par exemple, si le *délai de commande* propriété sur un **Recordset** ne seront pas visibles pour le fournisseur de données sous-jacent.

```

Recordset1.CursorLocation = adUseClient     'invokes cursor service
Recordset1.Open "authors", _
    "Provider=SQLOLEDB;Data Source=DBServer;User Id=MyUserID;" & _
    "Password=MyPassword;Initial Catalog=pubs;",,adCmdTable
Recordset1.Properties.Item("Command Time out") = 50
' 'Command Time out' property on DBServer is still default (30).

```

 Si votre application nécessite le Service de curseur, mais vous devez définir des propriétés dynamiques sur le fournisseur sous-jacent, définissez les propriétés avant d’appeler le Service de curseur. Paramètres de propriété d’objet commande sont toujours transmis au fournisseur de données sous-jacent, quelle que soit l’emplacement du curseur. Par conséquent, vous pouvez également utiliser un objet de commande pour définir les propriétés à tout moment.

> [!NOTE]
>  La propriété dynamique DBPROP_SERVERDATAONINSERT n’est pas prise en charge par le service de curseur, même si elle est prise en charge par le fournisseur de données sous-jacent.

|Nom de la propriété| Description|
|-------------------|-----------------|
|Recalcul automatique (DBPROP_ADC_AUTORECALC)|Pour les jeux d’enregistrements créés avec le Service de mise en forme des données, cette valeur indique la fréquence à laquelle les colonnes calculées et agrégées sont calculées. La valeur par défaut (valeur = 1) consiste à recalculer chaque fois que le Service de mise en forme de données détermine que les valeurs ont changé. Si la valeur est 0, les colonnes calculées ou d’agrégats sont calculées uniquement lorsque la hiérarchie est initialement créée.|
|Taille de lot (DBPROP_ADC_BATCHSIZE)|Indique le nombre d’instructions de mise à jour qui peuvent être regroupées avant d’être envoyées au magasin de données. Davantage d’instructions dans un lot, les moins d’allers-retours vers les données de magasin.|
|Mettre en cache les lignes enfants (DBPROP_ADC_CACHECHILDROWS)|Pour les jeux d’enregistrements créés avec le Service de mise en forme des données, cette valeur indique si les jeux d’enregistrements enfants est stockés dans un cache pour une utilisation ultérieure.|
|Version du moteur de curseur (DBPROP_ADC_CEVER)|Indique la version du Service de curseur utilisée.|
|Mettre à jour l’état de modification (DBPROP_ADC_MAINTAINCHANGESTATUS)|Indique le texte de la commande utilisée pour resynchroniser un une ou plusieurs lignes dans une jointure de plusieurs tables.|
|[Optimiser](../../../ado/reference/ado-api/optimize-property-dynamic-ado.md)|Indique si un index doit être créé. Lorsque la valeur **True**, autorise la création temporaire d’index pour améliorer l’exécution de certaines opérations.|
|[Modifier la forme nom](../../../ado/reference/ado-api/reshape-name-property-dynamic-ado.md)|Indique le nom de la **Recordset**. Peut être référencée dans actuel, ou ultérieure, commandes de mise en forme des données.|
|[Resynchronisation des commandes](../../../ado/reference/ado-api/resync-command-property-dynamic-ado.md)|Indique une chaîne de commande personnalisée qui est utilisée par le [Resync](../../../ado/reference/ado-api/resync-method.md) (méthode) lorsque le [Unique Table](../../../ado/reference/ado-api/unique-table-unique-schema-unique-catalog-properties-dynamic-ado.md) propriété est en vigueur.|
|[Catalogue unique](../../../ado/reference/ado-api/unique-table-unique-schema-unique-catalog-properties-dynamic-ado.md)|Indique le nom de la base de données contenant la table référencée dans la **Unique Table** propriété.|
|[Schéma unique](../../../ado/reference/ado-api/unique-table-unique-schema-unique-catalog-properties-dynamic-ado.md)|Indique le nom du propriétaire de la table référencée dans la **Unique Table** propriété.|
|[Table unique](../../../ado/reference/ado-api/unique-table-unique-schema-unique-catalog-properties-dynamic-ado.md)|Indique le nom de l’unique table dans un **Recordset** créé à partir de plusieurs tables qui peuvent être modifiées par les insertions, mises à jour ou les suppressions.|
|Critères de mise à jour (DBPROP_ADC_UPDATECRITERIA)|Indique quels champs de la **où** clause utilisés pour gérer les conflits qui se produisent pendant une mise à jour.|
|[Mettre à jour de la resynchronisation](../../../ado/reference/ado-api/update-resync-property-dynamic-ado.md) (DBPROP_ADC_UPDATERESYNC)|Indique si le **Resync** méthode est implicitement appelée après la [UpdateBatch](../../../ado/reference/ado-api/updatebatch-method.md) (méthode) (et son comportement), lorsque le **Unique Table** propriété est en vigueur.|

 Vous pouvez également définir ou extraire une propriété dynamique en spécifiant son nom comme index de la **propriétés** collection. Par exemple, obtenir et imprimer la valeur actuelle de la [optimiser](../../../ado/reference/ado-api/optimize-property-dynamic-ado.md) propriété dynamique, puis définissez une nouvelle valeur, comme suit :

```
Debug.Print rs.Properties("Optimize")
rs.Properties("Optimize") = True
```

## <a name="built-in-property-behavior"></a>Comportement de la propriété intégrée
 Le Service de curseur pour OLE DB affecte également le comportement de certaines propriétés intégrées.

|Nom de la propriété| Description|
|-------------------|-----------------|
|[CursorType](../../../ado/reference/ado-api/cursortype-property-ado.md)|Complète les types de curseurs disponibles pour un **Recordset**.|
|[LockType](../../../ado/reference/ado-api/locktype-property-ado.md)|Complète les types de verrous disponibles pour un **Recordset**. Active les mises à jour du lot.|
|[Sort](../../../ado/reference/ado-api/sort-property.md)|Spécifie le champ un ou plusieurs noms qui le **Recordset** est triée et si chaque champ est trié dans l’ordre croissant ou décroissant.|

## <a name="method-behavior"></a>Comportement de la méthode
 Le Service de curseur pour OLE DB Active ou affecte le comportement de la [champ](../../../ado/reference/ado-api/field-object.md) l’objet [Append](../../../ado/reference/ado-api/append-method-ado.md) méthode ; et le **Recordset** l’objet [ouvrir](../../../ado/reference/ado-api/open-method-ado-recordset.md), [Resync](../../../ado/reference/ado-api/resync-method.md), [UpdateBatch](../../../ado/reference/ado-api/updatebatch-method.md), et [enregistrer](../../../ado/reference/ado-api/save-method.md) méthodes.
