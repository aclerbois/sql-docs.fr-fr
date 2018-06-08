---
title: Données de la colonne | Documents Microsoft
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
- column data [ODBC]
- ODBC cursor library [ODBC], cache
- cursor library [ODBC], cache
- cache [ODBC]
ms.assetid: 0425818c-9469-493f-9e3c-fc03d9411c5c
caps.latest.revision: 8
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 428f7c4a9d0e21cb989721b095b65bf0c48306fe
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="column-data"></a>Données de la colonne
> [!IMPORTANT]  
>  Cette fonctionnalité sera supprimée dans une future version de Windows. Évitez d’utiliser cette fonctionnalité dans tout nouveau développement et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Microsoft recommande d’utiliser les fonctionnalités de curseur du pilote.  
  
 La bibliothèque de curseurs crée une mémoire tampon dans le cache pour chaque tampon de données lié à un jeu de résultats avec **SQLBindCol**. Il utilise les valeurs dans ces mémoires tampons pour construire un **où** clause lorsqu’il émule un positionnées mettre à jour ou supprimer l’instruction. Il met à jour ces mémoires tampons des tampons de lignes lorsqu’il extrait les données à partir de la source de données et quand il exécute les instructions de mise à jour positionnée.  
  
 Lorsque la bibliothèque de curseurs met à jour son cache de tampons de l’ensemble de lignes, il transfère les données en fonction du type de données C spécifiée dans **SQLBindCol**. Par exemple, si le type de données C d’une mémoire tampon d’ensemble de lignes est SQL_C_SLONG, la bibliothèque de curseurs transfère les quatre octets de données ; s’il s’agit de SQL_C_CHAR et *BufferLength* 10, la bibliothèque de curseurs transfère 10 octets de données. La bibliothèque de curseurs n’effectue pas la vérification du type ou les transformations sur les données que du transfert.  
  
> [!NOTE]  
>  La bibliothèque de curseurs ne met pas à jour son cache pour une colonne si **StrLen_or_IndPtr* dans l’ensemble de lignes correspondante mémoire tampon est SQL_DATA_AT_EXEC ou le résultat de la macro SQL_LEN_DATA_AT_EXEC.  
  
 Lorsqu’il met à jour une colonne, une source vide-remplit caractères de longueur fixe de données et les données binaires de longueur fixe remplit de zéro si nécessaire. Par exemple, une source de données stocke « Smith » dans une colonne char (10) en tant que « Smith ». La bibliothèque de curseurs n’effectue pas blancs ou zéro-remplissage des données dans les mémoires tampons de lignes lorsqu’il copie ces données à son cache après l’exécution d’une instruction de mise à jour positionnée. Par conséquent, si une application requiert que les valeurs dans le cache de la bibliothèque de curseurs sont complétées par des blancs ou de zéros, il doit blancs ou remplissage de zéro les valeurs dans les tampons de l’ensemble de lignes avant d’exécuter une instruction de mise à jour positionnée.
