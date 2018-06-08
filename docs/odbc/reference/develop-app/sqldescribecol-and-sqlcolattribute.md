---
title: SQLDescribeCol et SQLColAttribute | Documents Microsoft
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
- SQLColAttribute function [ODBC], and SQLDescribeCol
- SQLDescribeCol function [ODBC], and SQLColAttribute
- result sets [ODBC], metadata
- retrieving result set meta data [ODBC]
- metadata [ODBC], result set
ms.assetid: c2ca442c-03a8-4e0f-9e67-b300bb15962f
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: a99551fd76b68af9d48b5d97f8ef696259c8661e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqldescribecol-and-sqlcolattribute"></a>SQLDescribeCol et SQLColAttribute
**SQLDescribeCol** et **SQLColAttribute** sont utilisées pour récupérer les métadonnées du jeu de résultats. La différence entre ces deux fonctions est que **SQLDescribeCol** retourne toujours les cinq pièces d’informations (d’une colonne nom, type de données, précision, échelle et possibilité de valeur null), lors de la mêmes **SQLColAttribute** retourne un seul élément d’information demandé par l’application. Toutefois, **SQLColAttribute** peut retourner une sélection plus riche de métadonnées, notamment le respect de la casse d’une colonne, affiche la taille, les mises à jour et les possibilités de recherche.  
  
 De nombreuses applications, en particulier celles qui affichent uniquement les données, nécessitent uniquement les métadonnées retournées par **SQLDescribeCol**. Pour ces applications, il est plus rapide d’utiliser **SQLDescribeCol** à **SQLColAttribute** , car les informations sont retournées dans un seul appel. D’autres applications, notamment ceux qui mettent à jour des données, requièrent des métadonnées supplémentaires retournées par **SQLColAttribute** et donc pas utiliser les deux fonctions. En outre, **SQLColAttribute** prend en charge les métadonnées spécifiques au pilote ; pour plus d’informations, consultez [les Types de données spécifiques au pilote, Types de descripteur, Types d’informations, Types de diagnostics et attributs](../../../odbc/reference/develop-app/driver-specific-data-types-descriptor-information-diagnostic.md).  
  
 Une application peut récupérer les métadonnées du jeu de résultats à tout moment après une instruction a été préparée ou exécutée et avant le curseur sur le résultat d’ensemble est fermé. Très peu d’applications requièrent des métadonnées d’ensemble de résultats une fois que l’instruction est préparée et avant son exécution. Si possible, les applications doivent attendre de récupérer les métadonnées jusqu'à une fois que l’instruction est exécutée, car certaines sources de données ne peut pas retourner des métadonnées pour les instructions préparées et l’émulation de cette fonctionnalité dans le pilote est souvent un processus lent. Par exemple, le pilote peut générer des résultats d’une ligne zéro défini en remplaçant le **où** clause d’une **sélectionnez** instruction avec la clause **WHERE 1 = 2** et en exécutant l’instruction obtenue.  
  
 Les métadonnées sont souvent coûteuses à récupérer à partir de la source de données. Pour cette raison, les pilotes doivent mettre en cache des métadonnées à récupérer à partir du serveur et de la stocker pour tant que le curseur sur le résultat défini est ouvert. En outre, les applications doivent demander uniquement les métadonnées que dont ils ont absolument besoin.
