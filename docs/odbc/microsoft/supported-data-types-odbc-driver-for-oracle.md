---
title: Prise en charge des Types de données (le pilote ODBC pour Oracle) | Documents Microsoft
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
- data types [ODBC], ODBC driver for Oracle
- ODBC driver for Oracle [ODBC], data types
ms.assetid: 21d5f8d9-a3aa-4aa4-bc37-ff8bc90c0870
caps.latest.revision: 8
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 4f6e80b887659679992091a32faf2763d71d9f6f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="supported-data-types-odbc-driver-for-oracle"></a>Types de données pris en charge (pilote ODBC pour Oracle)
> [!IMPORTANT]  
>  Cette fonctionnalité sera supprimée dans une future version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Au lieu de cela, utilisez le pilote ODBC fourni par Oracle.  
  
 Le pilote ODBC pour Oracle prend en charge tous les types de données Oracle 7.3 ; Toutefois, il ne prend pas en charge les nouveaux types de données Oracle8 répertoriées ici.  
  
|Type de données|Oracle 7.3|Oracle8|  
|---------------|----------------|-------------|  
|BFILE|n/a|Non pris en charge|  
|BLOB|n/a|Non pris en charge|  
|CHAR|Pris en charge|Pris en charge|  
|CLOB|n/a|Non pris en charge|  
|DATE|Pris en charge|Pris en charge|  
|FLOAT|Pris en charge|Pris en charge|  
|INTEGER|Pris en charge|Pris en charge|  
|LONG|Pris en charge|Pris en charge|  
|LONG RAW|Pris en charge|Pris en charge|  
|NCHAR|n/a|Non pris en charge|  
|NCLOB|n/a|Non pris en charge|  
|NUMBER|Pris en charge|Pris en charge|  
|NVARCHAR2|n/a|Non pris en charge|  
|RAW|Pris en charge|Pris en charge|  
|VARCHAR2|Pris en charge|Pris en charge|  
|MLSLABEL|Non pris en charge.|Non pris en charge.|  
  
> [!NOTE]  
>  Pour plus d’informations sur la taille autorisée de la colonne VARCHAR, consultez [taille des colonnes VARCHAR](../../odbc/microsoft/varchar-column-size-odbc-driver-for-oracle.md) dans ce guide.
