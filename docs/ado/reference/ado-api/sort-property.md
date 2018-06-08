---
title: Propriété de tri | Documents Microsoft
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
- Recordset15::get_Sort
- Recordset15::put_Sort
- Recordset15::PutSort
- Recordset15::GetSort
- Recordset15::Sort
helpviewer_keywords:
- DESC [ADO]
- ASC [ADO]
- Sort property [ADO]
ms.assetid: 3683ffa0-6f93-4906-9533-ef6942f24f39
caps.latest.revision: 11
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 692a6a3e9ca2e65b031aebd8ed99c2719f0f0a69
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sort-property"></a>Propriété de tri
Indique un ou plusieurs noms de champ sur lequel le [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) est triée, et indique si chaque champ est trié dans l’ordre croissant ou décroissant.  
  
## <a name="settings-and-return-values"></a>Paramètres et valeurs de retour  
 Définit ou retourne un **chaîne** valeur qui indique le champ noms dans le **Recordset** sur laquelle effectuer le tri. Chaque nom est séparé par une virgule et peut être suivi d’un espace et le mot clé, **ASC**, laquelle trier le champ dans l’ordre croissant, ou **DESC**, laquelle trier le champ dans l’ordre décroissant. Par défaut, si aucun mot clé n’est spécifié, le champ est trié dans l’ordre croissant.  
  
## <a name="remarks"></a>Notes  
 Cette propriété requiert le [CursorLocation](../../../ado/reference/ado-api/cursorlocation-property-ado.md) propriété à définir **adUseClient**. Un index temporaire est créé pour chaque champ spécifié dans le **tri** si aucun index n’existe pas déjà une propriété.  
  
 L’opération de tri est efficace, car les données ne sont pas réorganisées physiquement, mais simplement accédées dans l’ordre spécifié par l’index.  
  
 Lorsque la valeur de la **tri** propriété n’est pas une chaîne vide, le **tri** ordre de la propriété a priorité sur l’ordre spécifié dans un **ORDER BY** clause inclus dans l’instruction SQL utilisée pour ouvrir le **Recordset**.  
  
 Le **Recordset** ne doit pas être ouverte avant d’accéder à la **tri** propriété ; elle peut être définie à tout moment après le **Recordset** objet est instancié.  
  
 Définition de la **tri** propriété à une chaîne vide sera réinitialise les lignes dans leur ordre d’origine et supprime les index temporaires. Les index existants ne seront pas supprimés.  
  
 Supposons qu’un **Recordset** contient trois champs nommés *firstName*, *middleInitial*, et *lastName*. Définir le **tri** à la chaîne de la propriété «`lastName DESC, firstName ASC`», sera le **Recordset** par nom dans l’ordre décroissant, puis par prénom par ordre croissant. L’initiale du milieu est ignoré.  
  
 Aucun champ peut être nommé « ASC » ou « DESC », car ces noms en conflit avec les mots clés **ASC** et **DESC**. Vous pouvez créer un alias pour un champ avec un nom en conflit à l’aide de la **AS** mot clé dans la requête qui retourne le **Recordset**.  
  
## <a name="applies-to"></a>S'applique à  
 [Recordset, objet (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Exemple de propriété de tri (VB)](../../../ado/reference/ado-api/sort-property-example-vb.md)   
 [Exemple de propriété de tri (VC ++)](../../../ado/reference/ado-api/sort-property-example-vc.md)   
 [Optimiser la propriété dynamique (ADO)](../../../ado/reference/ado-api/optimize-property-dynamic-ado.md)   
 [SortColumn, propriété (RDS)](../../../ado/reference/rds-api/sortcolumn-property-rds.md)   
 [SortDirection, propriété (RDS)](../../../ado/reference/rds-api/sortdirection-property-rds.md)
