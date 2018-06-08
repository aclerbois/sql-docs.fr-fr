---
title: Tableau d’état de la ligne | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- row status array [ODBC]
- cursors [ODBC], block
- result sets [ODBC], row status array
- block cursors [ODBC]
- result sets [ODBC], block cursors
- rowset status [ODBC]
ms.assetid: 4b69f189-2722-4314-8a02-f4ffecd6dabd
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: b2c45b2dc5ea9326b5ae3b229a17c13207edcabc
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="row-status-array"></a>Tableau d’état de ligne
En plus des données, **SQLFetch** et **SQLFetchScroll** peut retourner un tableau qui donne le statut de chaque ligne dans l’ensemble de lignes. Ce tableau est spécifié via l’attribut d’instruction SQL_ATTR_ROW_STATUS_PTR. Ce tableau est alloué par l’application et doit avoir autant d’éléments spécifiés par l’attribut d’instruction SQL_ATTR_ROW_ARRAY_SIZE. Les valeurs du tableau sont définies par **SQLBulkOperations**, **SQLFetch**, **SQLFetchScroll**, et **SQLSetPos.** Les valeurs de décrivent l’état de la ligne et si cet état a été modifiée depuis sa dernière extraction.  
  
|Valeur de tableau de statut de ligne| Description|  
|----------------------------|-----------------|  
|SQL_ROW_SUCCESS|La ligne a été extraite avec succès et n’a pas changé depuis sa dernière extraction.|  
|SQL_ROW_SUCCESS_WITH_INFO|La ligne a été extraite avec succès et n’a pas changé depuis sa dernière extraction. Toutefois, un avertissement a été retourné à la ligne.|  
|SQL_ROW_ERROR|Une erreur s’est produite lors de l’extraction de la ligne.|  
|SQL_ROW_UPDATED|La ligne a été extraite avec succès et a été mis à jour depuis sa dernière extraction. Si la ligne est extraite à nouveau ou actualisée par **SQLSetPos**, son état est passé à l’état nouveau.<br /><br /> Certains pilotes ne peut pas détecter les modifications apportées aux données et par conséquent impossible de retourner cette valeur. Pour déterminer si un pilote peut détecter les mises à jour à nouveau extraites selon les lignes, une application appelle **SQLGetInfo** avec l’option SQL_ROW_UPDATES.|  
|SQL_ROW_DELETED|La ligne a été supprimée depuis sa dernière extraction.|  
|SQL_ROW_ADDED|La ligne a été insérée par **SQLBulkOperations**. Si la ligne est extraite à nouveau ou qu’il est actualisée par **SQLSetPos**, son état est SQL_ROW_SUCCESS.<br /><br /> Cette valeur n’est pas définie **SQLFetch** ou **SQLFetchScroll**.|  
|SQL_ROW_NOROW|L’ensemble de lignes avec chevauchement de la fin du jeu de résultats, et aucune ligne n’a été retourné qui correspondait à cet élément du tableau d’état de ligne.|
