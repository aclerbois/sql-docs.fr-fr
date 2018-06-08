---
title: Détermination de la cible SGBD et les pilotes | Documents Microsoft
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
- target DBMSs and drivers in interoperability [ODBC]
- interoperability [ODBC], target dbmss and drivers
ms.assetid: 23bee0f6-e12a-4598-b34e-df11a8086829
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: fd4dc48994a028b2f8569da05511fa5c2b078165
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="determining-the-target-dbmss-and-drivers"></a>Détermination de la cible SGBD et les pilotes
La question suivante à prendre en compte est, quelles sont la SGBD cible pour l’application et les pilotes sont disponibles qui prennent en charge ces SGBD ? Étant donné que les applications génériques ont tendance à être hautement interopérable, la question de cible de SGBD est plus applicable à des applications personnalisées et verticales. Toutefois, la question de pilotes cibles s’applique à toutes les applications, car les pilotes peuvent varier dans la vitesse, la qualité, la prise en charge de la fonctionnalité et disponibilité. Également, si les pilotes doivent être redistribué par l’application, le coût et la disponibilité des régimes de licences en doivent être pris en compte.  
  
 Pour de nombreuses applications personnalisées, la cible de SGBD sont évidents : ils existent SGBD que l’application est conçue pour accéder à. SGBD auquel la future migration est planifiée est également à envisager. Toutefois, la question majeure pour ces applications est l’ou les pilotes à utiliser avec eux. Pour d’autres applications personnalisées : celles qui ne sont pas conçus pour accéder à un SGBD existant : la SGBD cible peut être choisie en fonction de la prise en charge de la fonctionnalité, prise en charge des utilisateurs simultanés, disponibilité du pilote et prix.  
  
 Pour les applications verticales, la cible de que SGBD est généralement choisis selon prise en charge de la fonctionnalité, disponibilité du pilote et marché. Par exemple, une application verticale conçue pour les petites entreprises doit cibler les SGBD qui s’adaptent à ces entreprises ; une application verticale conçue comme un module complémentaire pour les SGBD existants doit cibler largement utilisé SGBD.  
  
 Lorsque vous choisissez cible SGBD, les différences entre les bases de données de bureau et serveurs doivent être considérées. Bases de données bureautiques tels que dBASE, Paradox et Btrieve sont moins puissants que les bases de données de serveur. Car ils sont généralement accessibles via les moins puissants moteurs SQL trouvés dans les pilotes plus basés sur le fichier, souvent, ils ne disposent pas de prise en charge complète des transactions prend en charge un nombre d’utilisateurs simultanés et sont limitée à SQL. Toutefois, elles sont peu coûteux et ont une vaste base installée.  
  
 Bases de données de serveur telles que Oracle, DB2 et SQL Server prennent en charge complète des transactions prend en charge de nombreux utilisateurs simultanés et ont SQL riche. Elles sont beaucoup plus onéreuse et disposer d’une plus petite base installée. En revanche, prix du logiciel ont tendance à être plus élevée, quelque peu compensation un marché potentiel plus petit.  
  
 Par conséquent, vous pouvez choisir cible SGBD parfois selon les fonctionnalités requises par l’application et de marché de l’application cible. Par exemple, un système d’entrée de commande pour les grandes entreprises peuvent ne pas cible des bases de données bureautiques, car ceux-ci ne disposent pas de prise en charge des transactions adéquates. Un système semblable conçu pour les petites entreprises exclut peut-être la plupart des bases de données de serveur en fonction du coût. Et les développeurs d’applications génériques peuvent cibler à la fois, mais évitez d’utiliser les fonctionnalités avancées de bases de données de serveur.
