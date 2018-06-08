---
title: Propriétés dynamiques du jeu d’enregistrements au format XML | Documents Microsoft
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
- Recordset dynamic properties in XML [ADO]
ms.assetid: 52f8e379-812a-4db8-9210-94458926301c
caps.latest.revision: 3
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 56ce4985fddc55b6f3e3d204623c950a13953a86
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="recordset-dynamic-properties-in-xml"></a>Propriétés dynamiques du jeu d’enregistrements au format XML
Actuellement, les propriétés spécifiques au fournisseur de Recordset suivantes (à partir du moteur de curseur Client) sont conservées au format XML :  
  
-   Resynchronisation de la mise à jour  
  
-   Table unique  
  
-   Schéma unique  
  
-   Catalogue unique  
  
-   Resynchronisation des commandes  
  
-   IRowsetChange  
  
-   IRowsetUpdate  
  
-   CommandTimeout  
  
-   BatchSize  
  
-   UpdateCriteria  
  
-   Modifier la forme nom  
  
-   AutoRecalc  
  
 Ces propriétés sont enregistrées dans la section de schéma en tant qu’attributs de la définition d’élément du jeu d’enregistrements persistant. Ces attributs sont définis dans l’espace de noms de schéma d’ensemble de lignes et doit avoir le préfixe « rs : ».  
  
## <a name="see-also"></a>Voir aussi  
 [Persistance des enregistrements au format XML](../../../ado/guide/data/persisting-records-in-xml-format.md)
