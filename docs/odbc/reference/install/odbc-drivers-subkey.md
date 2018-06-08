---
title: Sous-clé de pilotes ODBC | Documents Microsoft
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
- subkeys [ODBC], drivers subkey
- registry entries for components [ODBC], drivers subkey
- drivers subkey [ODBC]
ms.assetid: 8edbf68f-d05d-4d77-92f6-e9500008f520
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 0359547122a9ee5537ae4634e6907e39f12916d0
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="odbc-drivers-subkey"></a>Sous-clé de pilotes ODBC
Les valeurs sous la sous-clé de pilotes ODBC répertorient les pilotes installés. Le format de ces valeurs est illustré dans le tableau suivant.  
  
|Nom|Type de données|data|  
|----------|---------------|----------|  
|*description du pilote*|REG_SZ|**Installé**|  
  
 Le *description du pilote* nom est défini par le développeur. Il s’agit généralement du nom du SGBD associé au pilote.  
  
 Par exemple, supposons que les pilotes ont été installés pour les fichiers de texte mis en forme et de SQL Server. Les valeurs sous la sous-clé de pilotes ODBC peuvent être :  
  
```  
Text : REG_SZ : Installed  
SQL Server : REG_SZ : Installed  
```
