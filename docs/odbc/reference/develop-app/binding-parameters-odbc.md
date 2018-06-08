---
title: Liaison de paramètres ODBC | Documents Microsoft
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
- binding parameters [ODBC]
ms.assetid: 7538a82b-b08b-4c8f-9809-e4ccea16db11
caps.latest.revision: 6
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: cc39c8183c996dd4d011d9bcdaba6dfe1f94cc05
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="binding-parameters-odbc"></a>Liaison de paramètres ODBC
Chaque paramètre dans une instruction SQL doit être associé, ou *lié,* à une variable dans l’application avant que l’instruction est exécutée. Lorsque l’application lie une variable à un paramètre, il décrit cette variable, adresse, type de données C et ainsi de suite, pour le pilote. Elle décrit également le paramètre lui-même : données SQL type, précision et ainsi de suite. Le pilote stocke ces informations dans la structure, il tient à jour pour cette instruction et utilise les informations pour récupérer la valeur de la variable lorsque l’instruction est exécutée.  
  
 Les paramètres peuvent être liés ou reliées à tout moment avant l’exécution d’une instruction. Si un paramètre est reliée après qu’une instruction est exécutée, la liaison ne s’applique pas jusqu'à ce que l’instruction est exécutée à nouveau. Pour lier un paramètre à une variable différente, une application relie simplement le paramètre avec la nouvelle variable ; la liaison précédente est automatiquement libérée.  
  
 Une variable est liée à un paramètre jusqu'à ce qu’une variable différente est liée au paramètre, jusqu'à ce que tous les paramètres sont déliés en appelant **SQLFreeStmt** avec l’option SQL_RESET_PARAMS, ou jusqu'à ce que l’instruction est libérée. Pour cette raison, l’application doit être sûr que les variables ne sont pas libérées tant qu’une fois qu’ils sont indépendants. Pour plus d’informations, consultez [affectation et la libération de mémoires tampons](../../../odbc/reference/develop-app/allocating-and-freeing-buffers.md).  
  
 Étant donné que les liaisons de paramètres sont simplement des informations stockées dans la structure maintenue par le pilote pour l’instruction, peuvent être définies dans n’importe quel ordre. Ils sont également indépendantes de l’instruction SQL qui est exécutée. Par exemple, une application lie les trois paramètres, puis l’exécute l’instruction SQL suivante :  
  
```  
INSERT INTO Parts (PartID, Description, Price) VALUES (?, ?, ?)  
```  
  
 Si l’application immédiatement exécute ensuite l’instruction SQL  
  
```  
SELECT * FROM Orders WHERE OrderID = ?, OpenDate = ?, Status = ?  
```  
  
 sur le même descripteur d’instruction, les liaisons de paramètres pour le **insérer** instruction sont utilisées, car ce sont les liaisons stockées dans la structure de l’instruction. Dans la plupart des cas, ceci est une pratique de programmation médiocre et doit être évitée. Au lieu de cela, l’application doit appeler **SQLFreeStmt** avec l’option SQL_RESET_PARAMS pour dissocier tous les paramètres de l’ancien, puis lier d’autres.  
  
 Cette section contient les rubriques suivantes.  
  
-   [Liaison de marqueurs de paramètre](../../../odbc/reference/develop-app/binding-parameter-markers.md)  
  
-   [Liaison de paramètres par nom (paramètres nommés)](../../../odbc/reference/develop-app/binding-parameters-by-name-named-parameters.md)  
  
-   [Décalages des liaisons de paramètres](../../../odbc/reference/develop-app/parameter-binding-offsets.md)  
  
-   [Description des paramètres](../../../odbc/reference/develop-app/describing-parameters.md)
