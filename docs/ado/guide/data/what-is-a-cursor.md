---
title: Qu’est un curseur ? | Microsoft Docs
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
- cursors [ADO], about cursors
ms.assetid: 596eb4b6-c22f-4cde-b23f-172dd66c3161
caps.latest.revision: 10
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 7b2f3311063c0a4aac92acaa6054b9f320c3754e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="what-is-a-cursor"></a>Qu’est un curseur ?
Les opérations réalisées dans une base de données relationnelle s'exécutent sur un ensemble complet de lignes. L'ensemble de lignes retourné par une instruction SELECT contient toutes les lignes satisfaisant aux conditions de la clause WHERE de l'instruction. Cet ensemble complet de lignes retournées par l'instruction est appelé ensemble de résultats. Les applications, en particulier ceux qui sont interactifs et en ligne, ne peut pas toujours fonctionner efficacement l’ensemble de résultats en tant qu’unité. Ces applications ont besoin d'un mécanisme leur permettant de travailler avec une seule ligne ou avec un petit bloc de lignes à la fois. Les curseurs sont une extension des ensembles de résultats et fournissent ce mécanisme.  
  
 Un curseur est implémenté par une bibliothèque de curseurs. Une bibliothèque de curseurs est un logiciel, souvent implémentée en tant que partie d’un système de base de données ou d’une API, d’accès aux données qui est utilisé pour gérer les attributs des données retournées à partir d’une source de données (un jeu de résultats). Ces attributs incluent la gestion d’accès concurrentiel, la position du jeu de résultats, nombre de lignes retournées et si vous pouvez déplacer vers l’avant ou vers l’arrière (ou les deux) à l’aide du résultat définir (capacité de défilement).  
  
 Un curseur effectue le suivi de la position dans le jeu de résultats et vous permet d’effectuer plusieurs opérations ligne par ligne, par rapport à un jeu de résultats, avec ou sans renvoi à la table d’origine. En d’autres termes, les curseurs conçus pour renvoyer un jeu de résultats basé sur les tables dans les bases de données. Le curseur est ainsi nommé car il indique la position actuelle dans le jeu de résultats, tout comme le curseur sur un écran de l’ordinateur indique la position actuelle.  
  
 Il est important de vous familiariser avec le concept de curseurs avant de passer aux spécificités de son utilisation dans ADO.  
  
 L’utilisation de curseurs, vous pouvez :  
  
-   Spécifiez le positionner sur des lignes spécifiques dans le jeu de résultats.  
  
-   Récupérer une ligne ou un bloc de lignes basé sur la position du jeu de résultats actuel.  
  
-   Modifier les données dans les lignes à la position actuelle dans le jeu de résultats.  
  
-   Définir les différents niveaux de sensibilité aux modifications de données effectuées par d’autres utilisateurs.  
  
 Par exemple, considérez une application qui affiche une liste de produits disponibles pour un acheteur potentiel. L’acheteur fait défiler la liste pour afficher les coûts et les détails du produit et sélectionne un produit à l’achat. Défilement supplémentaires et la sélection se produit pour le reste de la liste. Point de vue de l’acheteur, les produits apparaissent à la fois, mais l’application utilise un curseur de défilement pour parcourir et vers le bas jusqu'à l’ensemble de résultats.  
  
 Vous pouvez utiliser les curseurs de diverses manières :  
  
-   Avec aucune ligne.  
  
-   Avec certaines ou toutes les lignes dans une table unique.  
  
-   Avec certaines ou toutes les lignes des tables jointes de façon logique.  
  
-   En lecture seule ou être mis à jour au niveau du curseur ou le champ.  
  
-   En avant uniquement ou avec défilement complet.  
  
-   Avec le curseur situé sur le serveur.  
  
-   Sensibles aux modifications de table sous-jacente a provoqué par d’autres applications (telles que l’appartenance, tri, des insertions, des mises à jour et suppressions).  
  
-   Existant sur le serveur ou le client.  
  
 Les curseurs en lecture seule aider les utilisateurs à parcourir le jeu de résultats et les curseurs peuvent implémenter des mises à jour de la ligne en lecture/écriture. Curseurs complexes peuvent être définies avec les jeux de clés qui pointent vers les lignes de table de base. Bien que certains curseurs sont en lecture seule dans une direction vers l’avant, d’autres peuvent avancer et reculer et permettent une actualisation dynamique du jeu de résultats en fonction des modifications apportées par les autres applications à la base de données.  
  
 Pas toutes les applications doivent utiliser les curseurs de données access ou mise à jour. Certaines requêtes ne nécessitent pas de redirection de la ligne mise à jour à l’aide d’un curseur. Les curseurs doivent être les dernier recours pour récupérer des données, et, vous devez choisir le curseur dont l’impact le plus bas possible. Lorsque vous créez un jeu de résultats en utilisant une procédure stockée, le jeu de résultats n’est pas modifiable à l’aide du curseur modifier ou mettre à jour les méthodes.  
  
## <a name="concurrency"></a>Accès concurrentiel  
 Dans certaines applications multi-utilisateur, il est très important pour les données présentées à l’utilisateur final à jour que possible. Un exemple classique d’un tel système est un système de réservation de billet d’avion, où de nombreux utilisateurs peuvent être se disputent même siège sur un vol donné (et par conséquent, un enregistrement unique). Dans ce cas, la conception de l’application doit gérer les opérations simultanées sur un seul enregistrement.  
  
 Dans d’autres applications, accès simultané n’est pas aussi important. Dans ce cas, les frais à conserver que les données en cours à tout moment ne peut pas être justifiée.  
  
## <a name="position"></a>Position  
 Un curseur conserve également le suivi de la position actuelle dans un jeu de résultats. Considérer la position du curseur comme un pointeur vers l’enregistrement actif, similaire à la façon dont un tableau index pointe vers la valeur à cet emplacement précis dans le tableau.  
  
## <a name="scrollability"></a>Capacité de défilement  
 Le type de curseur utilisé par votre application affecte également la possibilité de déplacer vers l’avant et vers l’arrière dans les lignes d’un jeu de résultats ; Cela est parfois appelée la capacité de défilement. La possibilité d’avancer *et* revenir en arrière dans un résultat de jeu augmente la complexité du curseur et n’est donc plus coûteux à implémenter. Pour cette raison, vous devez demander un curseur avec cette fonctionnalité uniquement lorsque cela est nécessaire.
