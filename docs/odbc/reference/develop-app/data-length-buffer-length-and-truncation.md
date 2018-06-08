---
title: Longueur des données, la longueur de la mémoire tampon et la troncation | Documents Microsoft
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
- data buffers [ODBC], length
- data length [ODBC]
- truncating data [ODBC]
- length of data buffers [ODBC]
- buffers [ODBC], length
ms.assetid: 2825c6e7-b9ff-42fe-84fc-7fb39728ac5d
caps.latest.revision: 6
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 20d0e05e9739cbc8382a7a23c58b48084415990c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="data-length-buffer-length-and-truncation"></a>Longueur des données, la longueur de la mémoire tampon et la troncation
Le *longueur des données* est la longueur en octets des données telles qu’elles sont stockées dans le tampon de données de l’application, pas comme il est stocké dans la source de données. Cette distinction est importante, car les données sont souvent stockées dans des types différents dans la mémoire tampon de données que dans la source de données. Par conséquent, pour les données envoyées à la source de données, il s’agit la longueur en octets des données avant la conversion en type de la source de données. Pour les données récupérées à partir de la source de données, il est la longueur en octets des données après la conversion en type de la mémoire tampon les données et avant toute troncation.  
  
 Pour les données de longueur fixe, par exemple un entier ou une structure de date, la longueur en octets des données est toujours la taille du type de données. En règle générale, les applications d’allouer un tampon de données qui est la taille du type de données. Si l’application alloue une mémoire tampon plus petite, les conséquences ne sont pas définis, car le pilote suppose que le tampon de données est la taille du type de données et ne tronque pas les données pour tenir dans une mémoire tampon plus petite. Si l’application alloue une mémoire tampon plus importante, l’espace supplémentaire n’est jamais utilisé.  
  
 Pour les données de longueur variable, telles que binaire ou caractère, il est important de noter que la longueur en octets des données est souvent différent de la longueur d’octet de la mémoire tampon et séparé. La relation entre ces deux longueurs est décrite dans le [tampons](../../../odbc/reference/develop-app/buffers.md) section. Si la longueur en octets des données est supérieure à la longueur d’octet de la mémoire tampon, le pilote tronque les données extraites de la longueur d’octet de la mémoire tampon et retourne SQL_SUCCESS_WITH_INFO avec SQLSTATE 01004 (données tronquées). Toutefois, la longueur d’octet renvoyée est la longueur des données non tronquées.  
  
 Par exemple, qu'une application alloue de 50 octets pour une mémoire tampon de données binaires. Si le pilote a 10 octets de données binaires à retourner, il retourne les 10 octets dans la mémoire tampon. La longueur en octets des données est 10 et la longueur d’octet de la mémoire tampon est 50. Si le pilote a 60 octets de données binaires à retourner, elle tronque les données à 50 octets retourne ces octets dans la mémoire tampon et retourne SQL_SUCCESS_WITH_INFO. La longueur en octets des données est de 60 (longueur avant la troncature), et la longueur d’octet de la mémoire tampon reste encore 50.  
  
 Un enregistrement de diagnostic est créé pour chaque colonne est tronquée. Étant donné que de temps pour le pilote créer ces enregistrements et de l’application pour les traiter, la troncation peut dégrader les performances. En règle générale, une application peut éviter ce problème en allouant des tampons de grande taille suffisamment, bien que cela est peut-être pas possible lorsque vous travaillez avec des données de type long. En cas de troncation de données, l’application peut parfois allouer une mémoire tampon plus importante et extraire les données ; Cela n’est pas vrai dans tous les cas. Si une troncation se produit lors de l’obtention des données avec des appels à **SQLGetData**, l’application ne doive pas appeler **SQLGetData** pour les données qui ont déjà été retournées ; pour plus d’informations, consultez [obtention de données de type Long](../../../odbc/reference/develop-app/getting-long-data.md).
