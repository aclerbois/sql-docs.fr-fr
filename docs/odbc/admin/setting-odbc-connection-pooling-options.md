---
title: Définition des Options de mise en pool de connexions ODBC | Documents Microsoft
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
- connection pooling [ODBC]
- ODBC data source administrator [ODBC], connection pooling options
- ODBC data source administrator [ODBC], performance monitoring
ms.assetid: 037e2f78-f204-40f4-b4ab-d9cdf562012b
caps.latest.revision: 10
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: af8e9931c4026d578f226ec5e119815312c36a7a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="setting-odbc-connection-pooling-options"></a>Définition des Options de mise en pool de connexions ODBC
Le regroupement de connexions permet à une application d’utiliser une connexion à partir d’un pool de connexions qui n’ont pas besoin d’être rétablies à chaque utilisation. Vous pouvez utiliser la **le regroupement de connexions** onglet de la **administrateur de sources de données ODBC** boîte de dialogue pour activer et désactiver l’analyse des performances. Double-cliquez sur un nom de pilote pour définir la période de délai d’attente de connexion.  
  
 Au niveau du pilote, le regroupement de connexions est activé par la valeur de Registre CPTimeout. Ce pilote sélective l’activation permet à un administrateur système activer le regroupement de connexions pour simplement les pilotes qui peuvent prendre en charge. Il est effectué en affectant la valeur par défaut CPTimeout pendant le programme d’installation du pilote. Double-cliquez sur un nom de pilote pour définir la période de délai d’attente de connexion.  
  
 Pour plus d’informations sur le regroupement de connexions, consultez [le regroupement de connexions ODBC](../../odbc/reference/develop-app/driver-manager-connection-pooling.md).  
  
## <a name="performance-monitoring"></a>Analyse des performances  
 Analyse des performances assure le suivi des performances de connexion en enregistrant une série de statistiques. Ces statistiques peuvent être personnalisés par le développeur pour inclure les éléments suivants :  
  
|Compteur|Définition|  
|-------------|----------------|  
|Compteur de disque dur de connexion ODBC par seconde|Le nombre de connexions réelles par seconde qui sont effectuées sur le serveur. La première fois que votre environnement comporte une charge importante, ce compteur vont monter très rapidement. Après quelques secondes, il sera remise à zéro. Il s’agit de la situation normale lorsque le regroupement de connexions fonctionne. Lorsque les connexions au serveur ont été établies, elles seront utilisées et placées dans le pool pour une réutilisation.|  
|ODBC dur déconnecter compteur par seconde|Le nombre de disques durs déconnexions par seconde adressée au serveur. Il s’agit des connexions réelles sur le serveur qui sont publiées par le regroupement de connexions. Cette valeur augmente à partir de zéro lorsque vous arrêtez tous les clients sur le système et les connexions commencent à délai d’attente.|  
|Compteur de connexion logicielle ODBC par seconde|Le nombre de connexions satisfaits par le pool par seconde, en d’autres termes, les connexions à partir de ce pool qui ont été transmies aux utilisateurs. Ce compteur indique si le regroupement fonctionne. Selon la charge sur votre serveur, il n’est pas rare pour cette option pour afficher les connexions conditionnelles de 40 à 60 par seconde.|  
|Compteur de déconnexion logicielle ODBC par seconde|Nombre de déconnexions par seconde émis par les applications. Lorsque l’application libère ou se déconnecte, la connexion est placée dans le pool.|  
|Compteur de connexion Active actuelle ODBC|Le nombre de connexions dans le pool qui sont actuellement en cours d’utilisation.|  
|Compteur de connexion libre cours ODBC|Nombre actuel de connexions libres disponibles dans le pool. Il s’agit des connexions actives qui peuvent être utilisés.|  
|Des pools d’actif|Le nombre de pools actuellement actifs. Ce compteur a été ajouté dans Windows 8, pour les pilotes qui gèrent les connexions dans le pool de connexions. Pour plus d’informations, consultez [le regroupement de connexions prenant en charge les pilotes](../../odbc/reference/develop-app/driver-aware-connection-pooling.md).|  
|Pools créés|Le nombre de pools actives, y compris les pools actifs et supprimés. Ce compteur a été ajouté dans Windows 8, pour les pilotes qui gèrent les connexions dans le pool de connexions. Pour plus d’informations, consultez [le regroupement de connexions prenant en charge les pilotes](../../odbc/reference/develop-app/driver-aware-connection-pooling.md).|  
  
 Vous devez spécifier vos propres paramètres d’analyse. Exemples pour l’analyse des performances ont été inclus avec cette version d’ODBC.
