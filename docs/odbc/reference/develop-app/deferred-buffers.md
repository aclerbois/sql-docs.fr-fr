---
title: Différée tampons | Documents Microsoft
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
- buffers [ODBC], deferred
- deferred buffers [ODBC]
ms.assetid: 02c9a75c-2103-4f68-a1db-e31f7e0f1f03
caps.latest.revision: 6
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 14c1514759765699826c48a0cfb68e0eeba7a583
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="deferred-buffers"></a>Mémoires tampons différées
A *tampon différée* est celui dont la valeur est utilisée à un moment donné *après* elle est spécifiée dans un appel de fonction. Par exemple, **SQLBindParameter** est utilisé pour associer, ou *lier,* un tampon de données avec un paramètre dans une instruction SQL. L’application spécifie le numéro du paramètre et passe l’adresse, la longueur d’octet et le type de la mémoire tampon. Le pilote enregistre ces informations mais n’examine pas le contenu de la mémoire tampon. Plus tard, lorsque l’application s’exécute l’instruction, le pilote récupère les informations et l’utilise pour récupérer les données de paramètre et les envoyer à la source de données. Par conséquent, l’entrée de données dans la mémoire tampon est différée. Mémoires tampons différées sont spécifiés dans une fonction et utilisés dans un autre, il est une erreur de programmation d’application pour libérer une mémoire tampon différée, tandis que le pilote attend toujours qu’elle existe ; Pour plus d’informations, consultez [affectation et la libération de mémoires tampons](../../../odbc/reference/develop-app/allocating-and-freeing-buffers.md), plus loin dans cette section.  
  
 Tampons d’entrée et de sortie peuvent être différées. Le tableau suivant résume les utilisations des mémoires tampons différées. Notez que les tampons différées liés de façon à des colonnes de jeu sont spécifiées avec **SQLBindCol**, et les tampons différées liés aux paramètres de l’instruction SQL sont spécifiés avec **SQLBindParameter**.  
  
|Utilisation de la mémoire tampon|Type|Spécifié avec|Utilisée par|  
|----------------|----------|--------------------|-------------|  
|Envoi de données pour les paramètres d’entrée|Entrée différée|**SQLBindParameter**|**SQLExecute**<br /> **SQLExecDirect**|  
|Envoi de données à mettre à jour ou insérer une ligne dans un résultat défini|Entrée différée|**SQLBindCol**|**SQLSetPos**|  
|Renvoi de données pour les paramètres de sortie et d’entrée/sortie|Sortie différée|**SQLBindParameter**|**SQLExecute**<br /> **SQLExecDirect**|  
|Retour de résultats définie les données|Sortie différée|**SQLBindCol**|**SQLFetch**<br /> **SQLFetchScroll SQLSetPos**|
