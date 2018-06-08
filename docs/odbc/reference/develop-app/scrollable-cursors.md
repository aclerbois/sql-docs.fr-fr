---
title: Curseurs de défilement | Documents Microsoft
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
- scrollable cursors [ODBC]
- cursors [ODBC], scrollable
ms.assetid: 2c8a5f50-9b37-452f-8160-05f42bc4d97e
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 7e5c31c1c7629d0c339735b8777b42a15f9d880b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="scrollable-cursors"></a>Curseurs de défilement
Dans les applications modernes basée sur l’écran, l’utilisateur fait défiler vers le haut et vers l’avant les données. Pour de telles applications, il existe un problème de retour à une ligne extraite précédemment. L’une des possibilités consiste à fermer et rouvrir le curseur, puis extraire des lignes jusqu'à ce que le curseur atteint la ligne requise. Une autre possibilité consiste à lire le jeu de résultats, mettre en cache localement et implémenter le défilement dans l’application. Les deux possibilités fonctionnent bien uniquement avec les petits jeux de résultats, et la possibilité de ce dernier est difficile à implémenter. Une meilleure solution consiste à utiliser un *curseur permettant le défilement,* qui peut revenir en arrière et en avant dans le jeu de résultats.  
  
 A *curseur permettant le défilement* est couramment utilisé dans les applications modernes basée sur un écran dans lequel l’utilisateur fait défiler en arrière dans les données. Toutefois, applications doivent utiliser les curseurs permettant le défilement uniquement lorsque les curseurs avant uniquement ne produit pas le travail, comme les curseurs de défilement sont généralement plus coûteuses que les curseurs avant uniquement.  
  
 La possibilité de déplacer vers le haut déclenche une question non applicable pour les curseurs avant uniquement : un curseur de défilement doit détecter les modifications apportées aux lignes extraites précédemment ? Autrement dit, il doit pour détecter les lignes récemment insérées, supprimées et mis à jour ?  
  
 Ce problème se pose, car la définition d’un résultat de la valeur, l’ensemble de lignes qui répond à certains critères, ne précise pas lorsque les lignes sont vérifiées pour voir si elles correspondent aux critères, ni précise si lignes doivent contenir les mêmes données chaque fois qu’ils sont extraits. L’omission de l’ancienne rend possible pour les curseurs de défilement détecter si les lignes ont été insérés ou supprimés, tandis que cette dernière permet de détecter les données mises à jour.  
  
 La possibilité de détecter les modifications est parfois utile, parfois pas. Par exemple, une application de comptabilité a besoin d’un curseur qui ignore toutes les modifications ; Il est impossible si le curseur affiche les dernières modifications de l’équilibrage de la documentation. En revanche, un système de réservation des compagnies aériennes a besoin d’un curseur qui affiche les dernières modifications apportées aux données ; sans ce type de curseur, il doit en permanence actualiser la base de données pour afficher la disponibilité de vol plus récente.  
  
 Pour couvrir les besoins de différentes applications, ODBC définit quatre types de curseurs de défilement. Ces curseurs varient à la fois dans les dépenses et dans leur capacité à détecter les modifications apportées au résultat défini. Notez que si un curseur de défilement peut détecter les modifications apportées aux lignes, il peut uniquement détecter lorsqu’il tente d’extraire les lignes ; Il n’existe aucun moyen pour la source de données d’informer le curseur des modifications apportées aux lignes extraites en cours. Notez également que la visibilité des modifications est également contrôlée par le niveau d’isolation de transaction ; Pour plus d’informations, consultez [d’Isolation des transactions](../../../odbc/reference/develop-app/transaction-isolation.md).  
  
 Cette section contient les rubriques suivantes.  
  
-   [Types de curseurs avec défilement](../../../odbc/reference/develop-app/scrollable-cursor-types.md)  
  
-   [Utilisation des curseurs avec défilement](../../../odbc/reference/develop-app/using-scrollable-cursors.md)  
  
-   [Défilement relatif et absolu](../../../odbc/reference/develop-app/relative-and-absolute-scrolling.md)  
  
-   [Signets](../../../odbc/reference/develop-app/bookmarks-odbc.md)
