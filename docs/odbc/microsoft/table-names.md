---
title: Noms de tables | Documents Microsoft
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
- SQL grammar [ODBC], table names
- table names [ODBC]
ms.assetid: f7a5cb0a-3be7-4f46-82f9-64ffdbceaa9b
caps.latest.revision: 7
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 53942dbfedbc630962fe49675620c47ad7e32b2a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="table-names"></a>Noms de tables
Lorsque le dBASE, Microsoft Excel, Paradox, ou pilote est utilisé, les noms de table qui se produisent dans la clause FROM de SELECT ou DELETE, après la clause INTO dans INSERT et après la mise à jour, CREATE TABLE et DROP TABLE peuvent contenir un chemin d’accès valide, nom du serveur principal, fichier texte et extension de nom.  
  
 Utilisation d’un nom de table dans une instruction SQL ne prend pas en charge l’utilisation de chemins d’accès ou des extensions, mais accepte uniquement le nom de principal (par exemple, une EMP à partir de C:\ABC\EMP).  
  
 Noms de corrélation (alias) peuvent être utilisés. Par exemple :  
  
```  
SELECT *    
FROM C:\ABC\EMP T1    
WHERE T1.COL1 = 'aaa'  
```
