---
title: L’importance de l’emplacement du curseur | Documents Microsoft
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
- server-side cursors [ADO]
- cursors [ADO], client-side
- client-side cursors [ADO]
- cursors [ADO], server-side
ms.assetid: 70ef5b1c-0459-41a1-b796-031f61a29a8a
caps.latest.revision: 9
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 5699c8c7bc3ab1ed54d9411ff889e43e8cf334d5
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="the-significance-of-cursor-location"></a>L’importance de l’emplacement du curseur
Chaque curseur utilise des ressources temporaires pour contenir ses données. Ces ressources peuvent être en mémoire, un fichier d’échange de disque, les fichiers de disque temporaire ou même temporaire dans la base de données. Le curseur est appelé un *côté client* curseur lorsque ces ressources se trouvent sur l’ordinateur client. Le curseur est appelé un *côté serveur* curseur lorsque ces ressources se trouvent sur le serveur.  
  
## <a name="client-side-cursors"></a>Curseurs côté client  
 Dans ADO, appelez un curseur côté client en utilisant le **adUseClient CursorLocationEnum.** Avec un curseur côté client non-jeu de clés, le serveur envoie l’ensemble de résultats sur le réseau à l’ordinateur client. L’ordinateur client fournit et gère les ressources temporaires requises par le curseur et jeu de résultats. L’application côté client peut parcourir l’ensemble de résultats pour déterminer les lignes requises.  
  
 Statiques et pilotés par les curseurs côté client peuvent placer une charge importante sur votre station de travail s’ils comprennent trop de lignes. Alors que toutes les bibliothèques de curseurs sont capables de créer des curseurs avec des milliers de lignes, les applications conçues pour extraire ces ensembles de lignes de grande taille peuvent être lent. Il existe des exceptions, bien sûr. Pour certaines applications, un curseur côté client volumineux peut être parfaitement approprié et performances ne peut pas être un problème.  
  
 Avantage du curseur côté client est une réponse rapide. Une fois que le jeu de résultats a été téléchargé sur l’ordinateur client, la navigation des lignes est très rapide. Votre application est généralement plus évolutive avec des curseurs côté client car les besoins en ressources du curseur sont placées sur chaque client et non sur le serveur.  
  
## <a name="server-side-cursors"></a>Curseurs côté serveur  
 Dans ADO, appelez un curseur côté serveur en utilisant le **adUseServer CursorLocationEnum.** Avec un curseur côté serveur, le serveur gère le résultat à l’aide des ressources fournies par l’ordinateur du serveur. Le curseur côté serveur retourne uniquement les données demandées sur le réseau. Ce type de curseur peut parfois offrir de meilleures performances que le curseur côté client, en particulier dans les situations où un trafic réseau excessif est un problème.  
  
 Toutefois, il est important de souligner qu’un curseur côté serveur, au moins temporairement — consomme d’importantes ressources du serveur pour chaque client actif. Vous devez planifier en conséquence pour vous assurer que votre matériel de serveur est capable de gérer tous les curseurs côté serveur demandées par les clients actifs. En outre, un curseur côté serveur peut être lent, car elle fournit uniquement un accès ligne unique, aucun curseur de lot n’est disponible.  
  
 Les curseurs côté serveur sont utiles lors de l’insertion, la mise à jour ou supprimer des enregistrements. Avec les curseurs côté serveur, vous pouvez avoir plusieurs instructions actives sur la même connexion.
