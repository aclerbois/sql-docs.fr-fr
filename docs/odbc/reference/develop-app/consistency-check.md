---
title: Vérification de cohérence | Documents Microsoft
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
- descriptors [ODBC], consistency checks
- consistency checks [ODBC]
ms.assetid: deb80efa-ad1f-4ea5-b334-9817cd279e5c
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 7be399dc7f8fda9d5223fa6a1749e1849bc9ee88
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="consistency-check"></a>Vérification de cohérence
Une vérification de cohérence est effectuée par le pilote automatiquement chaque fois qu’une application définit le champ SQL_DESC_DATA_PTR du APD, ARD ou IPD. Chaque fois que ce champ est défini, le pilote vérifie que la valeur du champ SQL_DESC_TYPE et les valeurs applicables pour le champ SQL_DESC_TYPE dans le même enregistrement sont valides et cohérents.  
  
 Le champ SQL_DESC_DATA_PTR d’un IPD n’est pas défini normalement ; Toutefois, une application peut faire pour forcer une vérification de cohérence de champs IPD. La valeur définie pour le champ SQL_DESC_DATA_PTR de l’IPD n’est pas réellement stockée et ne peut pas être récupérée par un appel à **SQLGetDescField** ou **SQLGetDescRec**; le paramètre fait uniquement pour forcer la vérification de cohérence. Une vérification de cohérence ne peut pas être effectuée sur un IRD.  
  
 Pour plus d’informations sur la vérification de cohérence, consultez [SQLSetDescRec](../../../odbc/reference/syntax/sqlsetdescrec-function.md).
