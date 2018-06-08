---
title: Paramètres de l’instruction | Documents Microsoft
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
- statement parameters [ODBC]
ms.assetid: 58d5b166-2578-4699-a560-1f1e6d86c49a
caps.latest.revision: 7
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 3e3c7aa5880c80dff412a69d51cda42d24824e01
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="statement-parameters"></a>Paramètres d’instruction
A *paramètre* est une variable dans une instruction SQL. Par exemple, supposons qu'une table de pièces possède des colonnes nommées PartID, Description et le prix. Pour ajouter un article sans paramètres nécessiterait la construction d’une instruction SQL telle que :  
  
```  
INSERT INTO Parts (PartID, Description, Price) VALUES (2100, 'Drive shaft', 50.00)  
```  
  
 Bien que l’instruction suivante insère une nouvelle commande, il n’est pas une bonne solution pour une application de saisie de commandes, car les valeurs à insérer ne peut pas être codées en dur dans l’application. Une alternative consiste à construire l’instruction SQL en cours d’exécution, en utilisant les valeurs à insérer. Cela est également pas une bonne solution, en raison de la complexité de la construction d’instructions au moment de l’exécution. La meilleure solution consiste à remplacer les éléments de la **valeurs** clause un point d’interrogation ( ?), ou *des marqueurs de paramètre*:  
  
```  
INSERT INTO Parts (PartID, Description, Price) VALUES (?, ?, ?)  
```  
  
 Les marqueurs de paramètres sont ensuite liés à des variables d'applications. Pour ajouter une nouvelle ligne, l’application a uniquement définir les valeurs des variables et exécuter l’instruction. Le pilote extrait ensuite les valeurs actuelles des variables et les envoie à la source de données. Si l’instruction doit être exécutée plusieurs fois, l’application peut rendre le processus encore plus efficace en préparant l’instruction.  
  
 L’instruction ci-dessus peut être codé en dur dans une application d’entrée de commande pour insérer une nouvelle ligne. Toutefois, les marqueurs de paramètres ne sont pas limités aux applications verticales. Pour n’importe quelle application, ils facilitent la difficulté de construction d’instructions SQL en cours d’exécution en évitant les conversions vers et à partir du texte. Par exemple, l’ID du composant ci-dessus est probablement stockées dans l’application en tant qu’entier. Si l’instruction SQL est générée sans des marqueurs de paramètre, l’application doit convertir l’identificateur de partie de texte et la source de données doit le convertir en un entier. En utilisant un marqueur de paramètre, l’application peut envoyer l’ID du composant du pilote en tant qu’entier, ce qui est généralement peut envoyer à la source de données sous forme d’entier. Cela permet d’économiser deux conversions. Pour les valeurs de données de type long, il est très important, car les formes de texte de telles valeurs dépassent fréquemment la longueur autorisée d’une instruction SQL.  
  
 Les paramètres sont valides uniquement dans certains endroits dans les instructions SQL. Par exemple, ils ne sont pas autorisés dans la liste de sélection (la liste des colonnes doivent être retournées par une **sélectionnez** instruction), ni autorisés en tant que les deux opérandes d’un opérateur binaire, tels que le signe égal (=), car il est impossible de déterminer le type de paramètre. En règle générale, les paramètres sont valides uniquement dans les instructions de langage de Manipulation de données (DML) et non dans les instructions de langage de définition de données (DDL). Pour plus d’informations, consultez [des marqueurs de paramètre](../../../odbc/reference/appendixes/parameter-markers.md) dans l’annexe c : SQL grammaire.  
  
 Lorsque l’instruction SQL appelle une procédure, des paramètres nommée peut être utilisé. Les paramètres nommés sont identifiés par leurs noms, pas par leur position dans l’instruction SQL. Ils peuvent être liés par un appel à **SQLBindParameter**, mais le paramètre est identifié par le champ SQL_DESC_NAME de l’IPD (descripteur de paramètre d’implémentation), non par le *ParameterNumber* argument de **SQLBindParameter**. Elles peuvent également être liées en appelant **SQLSetDescField** ou **SQLSetDescRec**. Pour plus d’informations sur les paramètres nommés, consultez [Binding Parameters by Name (Named Parameters)](../../../odbc/reference/develop-app/binding-parameters-by-name-named-parameters.md), plus loin dans cette section. Pour plus d’informations sur les descripteurs, consultez [descripteurs](../../../odbc/reference/develop-app/descriptors.md).  
  
 Cette section contient les rubriques suivantes.  
  
-   [Paramètres de liaison](../../../odbc/reference/develop-app/binding-parameters-odbc.md)  
  
-   [Définition de valeurs de paramètres](../../../odbc/reference/develop-app/setting-parameter-values.md)  
  
-   [Envoi de données de type Long](../../../odbc/reference/develop-app/sending-long-data.md)  
  
-   [La récupération des paramètres de sortie par SQLGetData](../../../odbc/reference/develop-app/retrieving-output-parameters-using-sqlgetdata.md)  
  
-   [Paramètres de procédure](../../../odbc/reference/develop-app/procedure-parameters.md)  
  
-   [Tableaux de valeurs de paramètre](../../../odbc/reference/develop-app/arrays-of-parameter-values.md)
