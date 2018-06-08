---
title: Pourquoi ODBC a été créé ? | Microsoft Docs
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
- ODBC [ODBC], about ODBC
ms.assetid: ba6eb993-316b-4650-bab8-d76583c00e53
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 88b081ecb06f023154ab227aa57691a1ccee03e1
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="why-was-odbc-created"></a>Pourquoi ODBC a été créé ?
Historiquement, les entreprises utilisent un SGBD unique. Tous les accès de base de données a été effectuée via le serveur frontal de ce système ou les applications écrites pour utiliser exclusivement avec ce système. Toutefois, comme l’utilisation d’ordinateurs a augmenté et plus matériels et logiciels ont été rendues disponibles, sociétés démarré pour acquérir des SGBD différents. Les raisons sont nombreux : personnes ont acheté quel était moins coûteux, ce qui a été le plus rapide, ce qu’ils déjà connaissait, ce qui a été plus récent sur le marché, ce qui a fonctionné mieux pour une application unique. Pour d’autres raisons ont été réorganisations et les fusions, où les services qui avaient un SGBD unique avaient désormais plusieurs.  
  
 Le problème a fait l’objet encore plus complexe avec l’avènement des ordinateurs personnels. Ces ordinateurs dans un ensemble d’outils pour interroger, analyser et afficher les données, ainsi que d’un nombre de bases de données peu coûteux, facile à utiliser. Dès lors, une seule corporation était souvent les données dispersées dans une multitude de postes de travail, les serveurs et mini-ordinateurs, stockées dans diverses bases de données incompatibles et accessibles par un grand nombre de différents outils, peu de laquelle toutes les données y accéder.  
  
 Le dernier défi fournie avec l’arrivée de client/serveur informatique, qui vise à rendre l’utilisation la plus efficace des ressources de l’ordinateur. Les ordinateurs personnels peu coûteux (clients) se trouvent sur le bureau et fournir à la fois une représentation graphique frontale pour les données et un nombre d’outils à moindre coût, telles que feuilles de calcul, graphiques à des programmes et des générateurs de rapports. Mini-ordinateurs et macroordinateurs (serveurs) hébergent les SGBD, où ils peuvent utiliser leur puissance de calcul et d’un emplacement central pour fournir l’accès aux données rapide et coordonnée. Comment était le logiciel frontal pour la connexion à la base de données principale ?  
  
 Un problème similaire confrontée à des éditeurs de logiciels indépendants (ISV). Éditeurs de logiciel de base de données pour mini-ordinateurs et grands systèmes d’écriture ont été généralement contraints d’écrire une seule version d’une application pour chaque SGBD ou écrire du code de propres au SGBD pour chaque SGBD qu’ils veulent accéder. Écriture de logiciels pour ordinateurs personnels des fournisseurs eu à écrire des routines d’accès aux données pour chaque SGBD différent avec lequel ils voulaient utiliser. Cela signifie souvent une grande quantité de ressources ont été passé à l’écriture et la gestion d’accès aux données routines plutôt que les applications et les applications ont été vendues souvent pas sur leur qualité, mais si elles Impossible d’accès aux données dans un SGBD donné.  
  
 Si nécessaire par les deux ensembles de développeurs était un moyen d’accéder aux données dans le SGBD différents. Le groupe de macroordinateur et ses besoin d’une méthode pour fusionner les données de SGBD différents dans une application unique, alors que le groupe d’ordinateurs personnels si nécessaire, cette possibilité, ainsi que d’écrire une application unique qui est indépendante de tout un SGBD. En bref, les deux groupes nécessaires un moyen interopérable pour accéder aux données ; ils nécessitent ouvrir la connectivité de base de données.
