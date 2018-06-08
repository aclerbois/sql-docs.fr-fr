---
title: Exemple de Diagnostic de passerelles | Documents Microsoft
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
- diagnostic information [ODBC], examples
- gateway diagnostic [ODBC]
- error messages [ODBC], diagnostic messages
ms.assetid: e0695fac-4593-4b3d-8675-cb8f73dab966
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: d9d77f42b0941cecc8a17fe3b54ee91705af0666
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="gateways-diagnostic-example"></a>Exemple de Diagnostic de passerelles
Dans une architecture de passerelle, un pilote envoie des demandes à une passerelle qui prend en charge ODBC. La passerelle envoie les demandes à un SGBD. Comme il est le composant qui s’interface avec le Gestionnaire de pilotes, le pilote met en forme et retourne les arguments de **SQLGetDiagRec**.  
  
 Par exemple, si une passerelle à Rdb basé sur Oracle sur Microsoft Open Data Services et si Rdb n’a pas trouvé la table EMPLOYEE, la passerelle peut générer ce message de diagnostic :  
  
```  
"[42S02][-1][DEC][ODS Gateway][Rdb]%SQL-F-RELNOTDEF, Table EMPLOYEE is not defined "  
   "in schema."  
```  
  
 Étant donné que l’erreur s’est produite dans la source de données, la passerelle a ajouté un préfixe de l’identificateur de source de données ([Rdb]) pour le message de diagnostic. Étant donné que la passerelle a été le composant à la source de données, il ajouté préfixes pour son fournisseur [DEC (]) et un identificateur ([ODS passerelle]) pour le message de diagnostic. Il également ajouté la valeur SQLSTATE et le code d’erreur Rdb au début du message de diagnostic. Cela autorise à préserver la sémantique de sa propre structure de message et de toujours fournir les informations de diagnostic pour le pilote ODBC. Le pilote traite les informations d’erreur associées à l’instruction d’erreur par la passerelle.  
  
 Étant donné que le pilote de la passerelle est le composant qui s’interface avec le Gestionnaire de pilotes, il utilise le message de diagnostic précédent pour formater et renvoyer les valeurs suivantes à partir de **SQLGetDiagRec**:  
  
```  
SQLSTATE:         "42S02"  
Native Error:      -1  
Diagnostic Msg:   "[DEC][ODS Gateway][Rdb]%SQL-F-RELNOTDEF, Table EMPLOYEE is not "  
                  "defined in schema."  
```
