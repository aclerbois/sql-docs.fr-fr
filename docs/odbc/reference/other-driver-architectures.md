---
title: Autres Architectures pilote | Documents Microsoft
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
- drivers [ODBC], heterogeneous join engines
- drivers [ODBC], ODBC on the server
- ODBC architecture [ODBC], drivers
- heterogeneous join engines[ODBC]
- drivers [ODBC], middle component
ms.assetid: 1cad06ee-5940-4361-8d01-7d850db1dd66
caps.latest.revision: 8
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 52b51847dee6dbe59a7bb4495e0739d2ee08c005
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="other-driver-architectures"></a>Autres Architectures de pilote
Certains pilotes ODBC ne sont pas strictement conformes à l’architecture décrite précédemment. Cela peut être, car les pilotes de remplir des fonctions autres que ceux d’un pilote ODBC traditionnel ou ne sont pas des pilotes dans le sens normal.  
  
## <a name="driver-as-a-middle-component"></a>Pilote comme un composant central  
 Le pilote ODBC peut se trouver entre le Gestionnaire de pilotes et un ou plusieurs pilotes ODBC. Lorsque le pilote dans le milieu est capable de fonctionner avec plusieurs sources de données, il agit comme un répartiteur d’appels ODBC (ou appelle correctement traduites) à d’autres modules qui vous fait accéder aux sources de données. Dans cette architecture, le pilote dans le milieu prend sur une partie du rôle d’un gestionnaire de pilote.  
  
 Un autre exemple de ce type de pilote est un programme spy pour ODBC, qui intercepte et copie des fonctions ODBC échangées entre le Gestionnaire de pilotes et le pilote. Cette couche peut être utilisée pour émuler un pilote ou une application. Pour le Gestionnaire de pilotes, la couche semble être le pilote ; pour le pilote, la couche semble le Gestionnaire de pilotes.  
  
## <a name="heterogeneous-join-engines"></a>Moteurs de jointure hétérogène  
 Certains pilotes ODBC sont basées sur un moteur de requête pour effectuer des jointures hétérogènes. Dans une architecture d’un moteur de jointure hétérogène (voir l’illustration suivante), le pilote s’affiche à l’application, comme un pilote mais s’affiche à une autre instance du Gestionnaire de pilotes en tant qu’application. Ce pilote traite une jointure hétérogène à partir de l’application en appelant des instructions SQL distinctes dans les pilotes pour chaque base de données jointe.  
  
 ![Architecture d’un moteur de jointure hétérogène](../../odbc/reference/media/fig3-4.gif "fig3-4")  
  
 Cette architecture fournit une interface commune pour l’application d’accéder aux données à partir de différentes bases de données. Il peut utiliser une méthode courante pour récupérer les métadonnées, telles que des informations sur les colonnes spéciales (identificateurs de ligne), et elle peut appeler des fonctions de catalogue communes pour récupérer des informations de dictionnaire de données. En appelant la fonction ODBC **SQLStatistics**, par exemple, l’application peut récupérer des informations sur les index sur les tables à joindre, même si les tables sur deux bases de données distinctes. Le processeur de requêtes n’a pas à vous soucier de la façon dont les bases de données stockent les métadonnées.  
  
 L’application a également un accès standard aux types de données. ODBC définit les types de données SQL common mappés aux types de données propres au SGBD. Une application peut appeler **SQLGetTypeInfo** pour récupérer des informations sur les types de données sur les différentes bases de données.  
  
 Lorsque l’application génère une instruction de jointure hétérogène, le processeur de requêtes dans cette architecture d’analyser l’instruction SQL et génère ensuite les instructions SQL distinctes pour chaque base de données à joindre. À l’aide de métadonnées sur chaque pilote, le processeur de requêtes peut déterminer la jointure la plus efficace, intelligente. Par exemple, si l’instruction joint deux tables sur une base de données avec une table sur une autre base de données, le processeur de requêtes permettre joindre les deux tables sur la base d’un données avant de joindre le résultat avec la table à partir de la base de données.  
  
## <a name="odbc-on-the-server"></a>ODBC sur le serveur  
 ODBC (pilotes) peuvent être installés sur un serveur afin qu’ils peuvent être utilisés par les applications sur n’importe quel d’une série d’ordinateurs clients. Dans cette architecture (voir l’illustration suivante), un gestionnaire de pilotes et un seul pilote ODBC sont installés sur chaque client et un autre gestionnaire de pilotes et une série de pilotes ODBC sont installés sur le serveur. Cela permet à chaque accès client pour une variété de pilotes utilisées et gérées sur le serveur.  
  
 ![Architecture des pilotes ODBC sur un serveur](../../odbc/reference/media/fig3-5.gif "FIG3-5")  
  
 L’un des avantages de cette architecture sont la configuration et maintenance logicielle efficace. Pilotes doivent uniquement être mis à jour dans un seul endroit : sur le serveur. À l’aide de sources de données système, les sources de données peuvent être définies sur le serveur pour une utilisation par tous les clients. Les sources de données ne doivent pas être définies sur le client. Le regroupement de connexions peut être utilisé pour simplifier le processus par lequel les clients se connectent aux sources de données.  
  
 Le pilote sur le client est généralement une très faible qui transfère l’appel du Gestionnaire de pilotes au serveur. Son empreinte peut être beaucoup plus petite que les pilotes ODBC entièrement fonctionnels sur le serveur. Dans cette architecture, les ressources client peuvent être libérées si le serveur dispose de plus de puissance de calcul. En outre, vous peuvent améliorer l’efficacité et la sécurité de l’ensemble du système en installant des serveurs de sauvegarde et exécution de l’équilibrage de charge pour optimiser l’utilisation du serveur.
