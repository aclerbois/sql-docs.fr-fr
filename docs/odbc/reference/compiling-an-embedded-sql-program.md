---
title: Compilez un programme SQL incorporé | Documents Microsoft
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
- SQL [ODBC], embedded SQL
- SQL statements [ODBC], embedded SQL
- compiling embedded SQL programs [ODBC]
- embedded SQL [ODBC]
ms.assetid: 9e94146a-5b80-4a01-b586-1e03ff05b9ac
caps.latest.revision: 8
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 4e4b89a65475b35b50a968b9497c6d90574c6738
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="compiling-an-embedded-sql-program"></a>Compilez un programme SQL incorporé
Comme un programme SQL incorporé contient une combinaison d’instructions de langage SQL et l’hôte, il ne peut pas être soumis directement à un compilateur pour le langage hôte. Au lieu de cela, il est compilé dans un processus en plusieurs étapes. Bien que ce processus est différent du produit, les étapes sont à peu près les mêmes pour tous les produits.  
  
 Cette illustration montre les étapes nécessaires pour compiler un programme SQL incorporé.  
  
 ![Étapes de compilation d’un programme SQL incorporé](../../odbc/reference/media/pr02.gif "pr02")  
  
 Dans la compilation d’un programme SQL incorporé cinq étapes :  
  
1.  Le programme SQL incorporé est soumis à la précompilés SQL, un outil de programmation. Le précompilés analyse le programme, recherche les instructions et les traite. Une autre précompilé est requis pour chaque langage de programmation pris en charge par le SGBD. Produits de SGBD offrent généralement precompilers pour une ou plusieurs langues, y compris C, Pascal, COBOL, Fortran, Ada, PL / I et divers langages de l’assembly.  
  
2.  Le précompilés génère deux fichiers de sortie. Le premier fichier est le fichier source, sont supprimé de ses instructions SQL incorporées. À leur place, le précompilés remplace les appels aux routines SGBD propriétaires qui fournissent le moment de l’exécution de lien entre le programme et le SGBD. En règle générale, les noms et les séquences d’appel de ces routines sont connus uniquement le précompilé et du SGBD ; ils ne sont pas une interface publique pour le SGBD. Le deuxième fichier est une copie de toutes les instructions SQL incorporées dans le programme. Ce fichier est parfois appelé un module de requête de base de données, ou DBRM.  
  
3.  La sortie du fichier source à partir de la précompilé est soumise au compilateur standard pour l’hôte de langage (par exemple, un compilateur C ou COBOL) de programmation. Le compilateur traite le code source et génère le code de l’objet en tant que sa sortie. Notez que cette étape n’a rien à voir avec le SGBD ou SQL.  
  
4.  L’éditeur de liens accepte les modules de l’objet générés par le compilateur, les liens avec les différentes routines de bibliothèque et génère un programme exécutable. Les routines de bibliothèque liées dans le programme exécutable incluent les routines de SGBD propriétaires décrites à l’étape 2.  
  
5.  Le module de demande de base de données généré par le précompilé est soumis à un utilitaire de liaison spéciale. Cet utilitaire examine les instructions SQL, analyse, valide et optimise les, puis génère un plan d’accès pour chaque instruction. Le résultat est un plan d’accès pour l’ensemble du programme, qui représente une version exécutable des instructions SQL incorporées. L’utilitaire de liaison stocke le plan dans la base de données, en lui assignant généralement le nom de l’application qui l’utilise. Indique si cette étape a lieu au moment de l’exécution ou de temps de compilation dépend du SGBD.  
  
 Notez que les étapes permettant de compiler un programme SQL incorporé correspondent très proche de la procédure décrite précédemment dans [le traitement d’une instruction SQL](../../odbc/reference/processing-a-sql-statement.md). En particulier, notez que le précompilés sépare les instructions SQL à partir du code de langage hôte et l’utilitaire de liaison analyse et valide les instructions SQL et crée les plans d’accès. Dans le SGBD où l’étape 5 a lieu au moment de la compilation, les quatre premières étapes de traitement d’une instruction SQL lieu au moment de la compilation, tandis que la dernière étape (exécution) a lieu au moment de l’exécution. Cela a pour effet de rendre l’exécution des requêtes dans ces SGBD très rapide.
