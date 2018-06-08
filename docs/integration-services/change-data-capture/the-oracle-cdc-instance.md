---
title: Instance Oracle CDC | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: change-data-capture
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: ed71e8c4-e013-4bf2-8b6c-1e833ff2a41d
caps.latest.revision: 8
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: b7b0aba8e2f75ab25794ace6d28fc0be05d7bd23
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="the-oracle-cdc-instance"></a>Instance Oracle CDC
  L'instance Oracle CDC est un processus créé par le service de capture de données modifiées Oracle pour traiter les modifications capturées d'une seule base de données source Oracle. L’instance Oracle CDC récupère sa configuration à partir de la table **cdc.xdbcdc_config** et conserve son état dans la table **cdc.xdbcdc_state** . Ces tables font partie de la base de données CDC, qui définit l'instance Oracle CDC. Pour plus d'informations sur la base de données xdbcdc et les tables, consultez [The CDC Databases](../../integration-services/change-data-capture/working-with-the-oracle-cdc-service.md#BKMK_CDCdatabase).  
  
 La section suivante décrit les tâches effectuées par l'instance Oracle CDC :  
  
-   **Gestion de la vérification du démarrage du service**: une fois démarrée, l’instance de capture de données modifiées charge sa configuration à partir de la table **xdbcdc_config** et exécute une série de vérifications d’état qui garantissent que l’état persistant de l’instance de capture de données modifiées est cohérent et qu’elle peut commencer le traitement des modifications.  
  
-   **Préparation de la capture des modifications**: lorsque la vérification réussit, l'instance Oracle CDC analyse toutes les instances de capture actuellement définies et prépare les requêtes Oracle LogMiner et d'autres structures de prise en charge nécessaires pour la capture des modifications. En outre, l'instance Oracle charge de nouveau l'état interne de capture qui a été enregistré lors de la dernière exécution de l'instance Oracle CDC.  
  
-   **Capture des modifications à partir d'Oracle**: l'instance Oracle CDC extrait les modifications à partir d'Oracle au moyen de la fonctionnalité Oracle LogMiner, les trie en fonction de la validation de transaction, puis modifie le temps dans une transaction et les écrit dans les tables de modifications [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de la base de données CDC.  
  
-   **Gestion de l'arrêt du service**: le cycle de vie de l'instance Oracle CDC est géré par le service de capture de données modifiées Oracle. Lorsque l'instance Oracle CDC doit s'arrêter, elle effectue les tâches suivantes :  
  
    -   elle cesse de lire dans le journal des transactions Oracle ;  
  
    -   elle cesse d'écrire les transactions Oracle terminées dans la base de données CDC ;  
  
    -   elle attend 30 secondes (si nécessaire) jusqu'à ce que l'écriture de la transaction actuelle soit terminée dans la base de données CDC. Si plus de 30 secondes s'écoulent, l'écriture est annulée et la transaction est restaurée (pour une nouvelle tentative lors du redémarrage de l'instance de capture de données modifiées) ;  
  
    -   dans un thread distinct, elle écrit autant d’enregistrements mis en mémoire cache que possible dans la table de transactions intermédiaires pendant au plus 30 secondes (de la transaction la plus ancienne à la plus récente), puis elle met à jour la table **xdbcdc_state** et valide les modifications.  
  
-   **Gestion des modifications de configuration**: l’instance Oracle CDC est avertie des modifications de configuration par le service de capture de données modifiées ou la détection d’une nouvelle version dans la table **cdc.xdbcdc_config** . La plupart des modifications ne requièrent pas le redémarrage de l'instance Oracle CDC (par exemple, ajout ou suppression des instances de capture). Toutefois, certaines modifications, telles que la modification de la chaîne de connexion Oracle ou des informations d'identification d'accès requièrent le redémarrage de l'instance de capture de données modifiées.  
  
-   **Gestion de la récupération**: quand une instance Oracle CDC démarre, son état interne est restauré à partir des tables **xdbcdc_state** et **xdbcdc_staged_transactions** . Une fois l'état restauré, l'instance de capture de données modifiées s'exécute de manière habituelle.  
  
## <a name="see-also"></a> Voir aussi  
 [Gestion des erreurs](../../integration-services/change-data-capture/error-handling.md)  
  
  
