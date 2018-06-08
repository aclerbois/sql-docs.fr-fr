---
title: Exemple de Diagnostic de pilote SGBD | Documents Microsoft
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
- DBMS-based driver diagnostic [ODBC]
- diagnostic information [ODBC], examples
- error messages [ODBC], diagnostic messages
ms.assetid: a80d54b0-43ff-4dfd-b6cb-f4694a5ed765
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 2d2f26421a71edad627dcc4d89c854dbafa78766
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="dbms-based-driver-diagnostic-example"></a>Exemple de Diagnostic de pilote SGBD
Un pilote basés sur SGBD envoie des demandes à un SGBD et retourne des informations à l’application via le Gestionnaire de pilotes. Étant donné que le pilote est le composant qui s’interface avec le Gestionnaire de pilotes, il met en forme et retourne les arguments de **SQLGetDiagRec**.  
  
 Par exemple, si vous utilisez SQL/Services, un pilote Microsoft d’Oracle Rdb a rencontré un nom de curseur non valide, il peut retourner les valeurs suivantes à partir de **SQLGetDiagRec**:  
  
```  
SQLSTATE:         "34000"  
Native Error:      0  
Diagnostic Msg:   "[Microsoft][ODBC Rdb Driver]Invalid cursor name: EMPLOYEE_CURSOR."  
```  
  
 Étant donné que l’erreur s’est produite dans le pilote, ajouté préfixes pour le message de diagnostic pour le fournisseur ([Microsoft]) et le (pilote [ODBC Rdb]).  
  
 Si le SGBD n’a pas trouvé la table EMPLOYEE, le pilote peut mettre en forme et renvoyer les valeurs suivantes à partir de **SQLGetDiagRec**:  
  
```  
SQLSTATE:         "42S02"  
Native Error:      -1  
Diagnostic Msg:   "[Microsoft][ODBC Rdb Driver][Rdb] %SQL-F-RELNOTDEF, Table EMPLOYEE "  
                  "is not defined in schema."  
```  
  
 Étant donné que l’erreur s’est produite dans la source de données, le pilote ajouté un préfixe de l’identificateur de source de données ([Rdb]) pour le message de diagnostic. Étant donné que le pilote a été le composant à la source de données, il ajouté préfixes pour son fournisseur ([Microsoft]) et un identificateur ([Rdb le pilote ODBC]) pour le message de diagnostic.
