---
title: Modifications du contrôle à la Table de Base de l’ensemble d’enregistrements (ADO) | Documents Microsoft
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
helpviewer_keywords:
- Unique Table property [ADO]
- Unique Schema property [ADO]
- Unique Catalog property [ADO]
ms.assetid: d0e775d8-e353-46a1-ad10-ed4cc240dfaa
caps.latest.revision: 12
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 76bf7ee2916e6fa4277154d7261f40b9b5959ea9
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="unique-table-unique-schema-unique-catalog-properties-dynamic-ado"></a>Table unique, de schéma Unique, Unique catalogue des propriétés dynamiques (ADO)
Vous permet de contrôle précis des modifications apportées à une table de base de données dans un [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) qui a été formé par une opération de jointure sur plusieurs tables de base.  
  
-   **Table unique** Spécifie le nom de la table de base sur laquelle les mises à jour, insertions et suppressions sont autorisées.  
  
-   **Schéma unique** Spécifie le *schéma*, ou le nom du propriétaire de la table.  
  
-   **Catalogue unique** Spécifie le *catalogue*, ou le nom de la base de données contenant la table.  
  
## <a name="settings-and-return-values"></a>Paramètres et valeurs de retour  
 Définit ou retourne un **chaîne** valeur qui est le nom d’une table, un schéma ou un catalogue.  
  
## <a name="remarks"></a>Notes  
 La table de base souhaitée est identifiée par son catalogue, schéma et les noms de table. Lorsque le **Unique Table** propriété est définie, les valeurs de la **schéma Unique** ou **Unique Catalog** propriétés permettent de trouver la table de base. Il est prévu, mais pas obligatoire, qu’une ou les deux le **schéma Unique** et **Unique Catalog** propriétés être définie avant le **Unique Table** est définie.  
  
 La clé primaire de la **Unique Table** est traité comme la clé primaire de l’ensemble du **Recordset**. Il s’agit de la clé qui est utilisée pour toutes les méthodes nécessitant une clé primaire.  
  
 Alors que **Unique Table** est défini, le [supprimer](../../../ado/reference/ado-api/delete-method-ado-recordset.md) méthode affecte uniquement la table nommée. Le [AddNew](../../../ado/reference/ado-api/addnew-method-ado.md), [Resync](../../../ado/reference/ado-api/resync-method.md), [mise à jour](../../../ado/reference/ado-api/update-method.md), et [UpdateBatch](../../../ado/reference/ado-api/updatebatch-method.md) méthodes affectent toutes les tables de base sous-jacentes appropriées de la **Recordset**.  
  
 **Table unique** doit être spécifié avant d’effectuer des resynchronisations personnalisées. Si **Unique Table** n’a pas été spécifié, le [Resync Command](../../../ado/reference/ado-api/resync-command-property-dynamic-ado.md) propriété n’a aucun effet.  
  
 Une erreur d’exécution des résultats si une table de base unique ne peut pas être trouvée.  
  
 Ces propriétés dynamiques sont ajoutées à la **Recordset** objet [propriétés](../../../ado/reference/ado-api/properties-collection-ado.md) collection lorsque le [CursorLocation](../../../ado/reference/ado-api/cursorlocation-property-ado.md) est définie sur  **adUseClient**.  
  
## <a name="applies-to"></a>S'applique à  
 [Recordset, objet (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Recordset, objet (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md)
