---
title: L’accès concurrentiel optimiste | Documents Microsoft
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
- transactions [ODBC], concurrency control
- concurrency control [ODBC]
- optimistic concurrency [ODBC]
ms.assetid: 9d71e09e-bc68-4c1f-9229-ed2a7be7d324
caps.latest.revision: 6
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 95593c35e55f899c3fb062adf4f5edde197f813c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="optimistic-concurrency"></a>Accès concurrentiel optimiste
*L’accès concurrentiel optimiste* tire son nom de l’hypothèse optimiste que collisions entre les transactions seront produit rarement ; une collision est dite se sont produites lors d’une autre transaction met à jour ou supprime une ligne de données entre le moment où il est en lecture par la transaction en cours et le temps, il est mis à jour ou supprimé. Il est l’opposé de *d’accès concurrentiel pessimiste,* ou de verrouillage, dans lequel le développeur d’applications estime que ces conflits sont courants.  
  
 Dans l’accès concurrentiel optimiste, une ligne est restée déverrouillé jusqu'à ce qu’il est temps de mettre à jour ou supprimer. À ce stade, la ligne est relue et vérifiée pour voir si elle a été modifié depuis sa dernière lecture. Si la ligne a changé, Échec de la mise à jour ou de suppression et doit être tentée à nouveau.  
  
 Pour déterminer si une ligne a été modifiée, la nouvelle version est vérifiée par rapport à une version mise en cache de la ligne. La vérification peut être basée sur la version de ligne, telles que la colonne timestamp dans SQL Server, ou les valeurs de chaque colonne dans la ligne. Nombreux SGBD ne gère pas les versions de ligne.  
  
 L’accès concurrentiel optimiste peut être implémentée par la source de données ou de l’application. Dans les deux cas, l’application doit utiliser un niveau d’isolation de transaction basse tels que Read Committed ; à l’aide d’un niveau supérieur inverse la concurrence accrue acquise à l’aide de l’accès concurrentiel optimiste.  
  
 Si l’accès concurrentiel optimiste est implémentée par la source de données, l’application définit l’attribut d’instruction SQL_ATTR_CONCURRENCY SQL_CONCUR_ROWVER ou SQL_CONCUR_VALUES. Pour mettre à jour ou supprimer une ligne, il exécute une mise à jour positionnée ou d’une instruction delete ou d’appels **SQLSetPos** comme il le ferait avec l’accès concurrentiel pessimiste ; la source de données ou le pilote retourne SQLSTATE 01001 (conflit d’opération de curseur) si la mise à jour ou la suppression échoue en raison d’une collision.  
  
 Si l’application elle-même implémente l’accès concurrentiel optimiste, il définit l’attribut d’instruction SQL_ATTR_CONCURRENCY à SQL_CONCUR_READ_ONLY pour lire une ligne. Si elle compare les versions de ligne et que vous ne connaissez pas la colonne de version de ligne, il appelle **SQLSpecialColumns** avec l’option SQL_ROWVER pour déterminer le nom de cette colonne.  
  
 L’application met à jour ou supprime la ligne en augmentant la concurrence pour SQL_CONCUR_LOCK (pour accéder en écriture à la ligne) et l’exécution d’une **mise à jour** ou **supprimer** instruction avec un **où** clause qui spécifie la version ou les valeurs de la ligne avait lors de l’application de la lecture. Si la ligne a changé depuis, l’instruction échoue. Si le **où** clause n’identifie pas la ligne, l’instruction peut également mettre à jour ou supprimer des autres lignes ; les versions de ligne soit toujours identifient des lignes, mais les valeurs de ligne identifient lignes uniquement si elles incluent la clé primaire.
