---
title: Préparation et l’exécution de commandes | Documents Microsoft
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
- Command object [ADO], preparing and executing commands
ms.assetid: 7448d9ee-7f4b-47e3-be54-2df8c9bbac32
caps.latest.revision: 11
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: d4d0736e2c4f0a09ec43f2e910cc7fd8c9263fc6
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="preparing-and-executing-commands"></a>Préparation et l’exécution de commandes
Les commandes sont émises pour un fournisseur des instructions pour effectuer certaines opérations sur la source de données sous-jacente. Une instruction SQL, par exemple, est une commande au fournisseur de données Microsoft SQL. Dans ADO, les commandes sont généralement représentées par **commande** des objets, bien que les commandes simples peuvent également être émis via **connexion** ou **Recordset** objets.  
  
 Vous pouvez utiliser la **commande** objet pour demander de n’importe quel type pris en charge de l’opération à partir du fournisseur, en supposant que le puisse interpréter correctement la chaîne de commande. Une opération courante pour les fournisseurs de données est d’interroger une base de données et retourner des enregistrements dans une **Recordset** objet, qui peut être considéré comme un conteneur pour stocker le résultat et un outil pour afficher le résultat. Comme avec de nombreux objets ADO, certains **commande** collections d’objets, des méthodes ou propriétés peuvent générer des erreurs lorsque référencé, selon les fonctionnalités du fournisseur.  
  
 Outre l’utilisation de **commande** objets, vous pouvez utiliser la **Execute** méthode sur le **connexion** objet ou le **ouvrir** sur la (méthode) **Jeu d’enregistrements** objet à émettre une commande et qu’il soit exécuté. Toutefois, vous devez utiliser un **commande** si vous devez réutiliser une commande dans votre code, ou si vous avez besoin passer des informations de paramétrage avec votre commande de l’objet. Ces scénarios sont présentés plus en détail plus loin dans cette section.  
  
> [!NOTE]
>  Certaines **commande**peuvent renvoyer un jeu de résultats sous forme de flux binaire ou en tant qu’un seul **enregistrement** plutôt que comme un **Recordset**, si cela est pris en charge par le fournisseur. En outre, certains **commande**s ne sont pas destinés à renvoyer de jeu de résultats (par exemple, une requête de mise à jour SQL). Cette section décrit le scénario le plus courant, cependant : l’exécution de **commande**qui retournent des résultats comme un **Recordset** objet. Pour plus d’informations sur le renvoi des résultats dans **enregistrement**s ou **flux**s, consultez [enregistrements et flux](../../../ado/guide/data/records-and-streams.md).  
  
 Cette section contient les rubriques suivantes.  
  
-   [Vue d’ensemble de l’objet Command](../../../ado/guide/data/command-object-overview.md)  
  
-   [Création et exécution d’une commande simple](../../../ado/guide/data/creating-and-executing-a-simple-command.md)  
  
-   [Paramètres de l’objet Command](../../../ado/guide/data/command-object-parameters.md)  
  
-   [Appel d’une procédure stockée avec une commande](../../../ado/guide/data/calling-a-stored-procedure-with-a-command.md)  
  
-   [Appel d’une procédure stockée en tant que méthode sur un objet de connexion](../../../ado/guide/data/calling-a-stored-procedure-as-a-method-on-a-connection-object.md)  
  
-   [Commandes nommées](../../../ado/guide/data/named-commands.md)  
  
-   [Passage de paramètres à une commande nommée](../../../ado/guide/data/passing-parameters-to-a-named-command.md)
