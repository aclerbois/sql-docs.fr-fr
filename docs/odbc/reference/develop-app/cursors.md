---
title: Curseurs | Microsoft Docs
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
- forward-only cursors [ODBC]
- scrollable cursors [ODBC]
- cursors [ODBC], about cursors
- cursors [ODBC]
- fetches [ODBC], cursors
- result sets [ODBC], fetching
- block cursors [ODBC]
ms.assetid: 0b114352-3c63-4d33-9220-182ede90e4aa
caps.latest.revision: 6
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 569714d6cd523498adddbda26576cf376af15c81
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="cursors"></a>Curseurs
Une application extrait les données avec un *curseur*. Un curseur est différent à partir d’un jeu de résultats : un jeu de résultats est l’ensemble de lignes qui correspond aux critères de recherche spécifique, tandis qu’un curseur est le logiciel qui retourne les lignes à l’application. Le nom *curseur,* telle qu’elle s’applique aux bases de données, probablement l’origine à partir du curseur clignotant sur un ordinateur Terminal Server. Comme ce curseur indique la position actuelle sur l’écran et l’emplacement où les mots apparaîtra suivants, un curseur sur un jeu de résultats indique la position actuelle dans le jeu de résultats et reviendrez ensuite les lignes.  
  
 Le modèle de curseur dans ODBC est basé sur le modèle de curseur dans embedded SQL. Une différence notable entre ces modèles est que le moyen de curseurs est ouverts. Dans embedded SQL, un curseur doit être explicitement déclaré et ouverts avant de pouvoir être utilisé. Dans ODBC, un curseur est ouvert lors de l’exécution d’une instruction qui crée un jeu de résultats. Lorsque le curseur est ouvert, il est positionné avant la première ligne du jeu de résultats. Dans embedded SQL et ODBC, un curseur doit être fermé une fois que l’application a fini de l’utiliser.  
  
 Curseurs différents ont des caractéristiques différentes. Le type le plus courant de curseur, ce qui est appelé un *curseur avant uniquement,* peut uniquement avancer dans le jeu de résultats. Pour revenir à une ligne précédente, l’application doit fermer et rouvrir le curseur et lire ensuite les lignes à partir du début du jeu jusqu'à ce qu’il atteigne la ligne requise de résultats. Les curseurs avant uniquement fournissent un mécanisme rapide pour une seule passe via un jeu de résultats.  
  
 Les curseurs avant uniquement sont moins utile pour les applications basées sur un écran dans lequel l’utilisateur fait défiler vers le haut et vers l’avant les données. Ces applications peuvent utiliser un curseur avant uniquement en lisant les résultats une fois la mise en cache les données localement et effectuer eux-mêmes de défilement. Toutefois, cela fonctionne bien uniquement avec petites quantités de données. Une meilleure solution consiste à utiliser un *curseur permettant le défilement,* qui fournit un accès aléatoire pour le jeu de résultats. De telles applications peuvent également accroître les performances en extraire plusieurs lignes de données à la fois, à l’aide de ce que l'on appelle un *curseur de bloc.* Pour plus d’informations sur les curseurs de bloc, consultez [à l’aide de curseurs de bloc](../../../odbc/reference/develop-app/using-block-cursors.md).  
  
 Le curseur avant uniquement est le type de curseur par défaut dans ODBC et est décrit dans les sections suivantes. Pour plus d’informations sur les curseurs de bloc et les curseurs de défilement, consultez [curseurs de bloc](../../../odbc/reference/develop-app/block-cursors.md) et [curseurs permettant le défilement](../../../odbc/reference/develop-app/scrollable-cursors.md).  
  
> [!IMPORTANT]  
>  Validation ou la restauration d’une transaction en appelant explicitement **SQLEndTran** ou par fonctionne en mode de validation automatique, fait que certaines sources de données fermer tous les curseurs sur toutes les instructions sur une connexion. Pour plus d’informations, consultez les attributs SQL_CURSOR_COMMIT_BEHAVIOR et SQL_CURSOR_ROLLBACK_BEHAVIOR dans les [SQLGetInfo](../../../odbc/reference/syntax/sqlgetinfo-function.md) description de fonction.
