---
title: Exemple de jeu d’enregistrements pour l’examen des données | Documents Microsoft
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
- record location [ADO]
- current record [ADO]
ms.assetid: e770e626-68b1-4ddf-a217-d7b30311e2ee
caps.latest.revision: 13
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: ca05161aad6aaaf79f1e1af85d7e83d55e6997ad
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sample-recordset-for-examining-data"></a>Jeu d’enregistrements de l’exemple pour l’examen des données
Tout d’abord, examinons la **Recordset** de l’objet retourné à l’aide de la requête SQL suivante, exécutée sur les données d’exemple Northwind base dans Microsoft SQL Server.  
  
```  
SELECT ProductID,ProductName,UnitPrice   
FROM Products   
WHERE CategoryID = 7    
```  
  
 Résultant **Recordset** objet contient tous les génère dans la base de données indiqué dans le tableau suivant.  
  
|ProductID|ProductName|UnitPrice|  
|---------------|-----------------|---------------|  
|7|Poires secs organiques d’oncle Bob|30.0000|  
|14|Tofu|23.2500|  
|28|Rssle choucroute|45.6000|  
|51|Manjimup secs pommes|53.0000|  
|74|Longlife Tofu|10.0000|  
  
 Si vous êtes intéressé de l’obtention de ces résultats, essayez l’exemple JScript suivant :  
  
-   [Exemple JScript pour retourner un jeu d’enregistrements](../../../ado/guide/data/jscript-code-example-to-return-a-recordset.md)
