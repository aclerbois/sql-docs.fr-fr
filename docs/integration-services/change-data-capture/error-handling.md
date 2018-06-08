---
title: Gestion des erreurs | Microsoft Docs
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
ms.assetid: ff79e19d-afca-42a4-81b0-62d759380d11
caps.latest.revision: 12
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: bd409daab0e317cff3ad474bda2963115caa1c60
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="error-handling"></a>Gestion des erreurs
  Une instance Oracle CDC exploite les modifications d'une base de données source Oracle (un cluster Oracle RAC est considéré comme une seule base de données) et écrit les modifications validées dans les tables de modifications dans une base de données CDC de l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] cible.  
  
 Une instance de capture de données modifiées conserve son état dans une table système appelée **cdc.xdbcdc_state**. Cette table peut être interrogée à tout moment pour trouver l'état de l'instance de capture de données modifiées. Pour plus d’informations sur la table cdc.xdbcdc_state, consultez [cdc.xdbcdc_state](../../integration-services/change-data-capture/the-oracle-cdc-databases.md#BKMK_cdcxdbcdc_state).  
  
 Le tableau ci-dessous décrit les états de l'instance de capture de données modifiées dans la table xdbcdc_state.  
  
 Pour chaque état, les deux indications suivantes s'affichent pour les colonnes correspondantes dans la table cdc.xdbcdc_state :  
  
-   L'instance n'est pas active (elle n'est actuellement gérée par aucun processus Windows). Si la valeur de la colonne **active** est 1, un sous-processus du service de capture de données modifiées Oracle gérant cette instance Oracle CDC spécifique est en cours d’exécution.  
  
-   Si la valeur de la colonne **d’erreur** est 0, l’instance Oracle CDC n’est pas dans une condition d’erreur. Si la valeur de la colonne **d’erreur** est 1, il existe une erreur qui empêche l’instance Oracle CDC de traiter les modifications.  
  
     Si la colonne **d’erreur** a la valeur 1 et la valeur de la colonne **active** est également 1, une erreur récupérable se produit pour l’instance Oracle CDC, qui peut être résolue automatiquement. Si la colonne d'erreur a la valeur 1 et la colonne active a la valeur 0, dans la plupart des cas une solution de contournement manuelle peut être nécessaire pour résoudre le problème avant la reprise du traitement.  
  
 Le tableau suivant décrit les différents codes d'état que l'instance Oracle CDC peut signaler dans sa table d'état.  
  
|État|Code d'état actif|Code d'état d'erreur|Description|Sous-état|  
|------------|------------------------|-----------------------|-----------------|---------------|  
|ABORTED|0| 1|L'instance Oracle CDC ne s'exécute pas. Le sous-état ABORTED indique que l'instance Oracle CDC était ACTIVE, puis s'est arrêtée de façon inattendue.|Le sous-état ABORTED est établi par l'instance principale de service de capture de données modifiées Oracle lorsqu'elle détecte que l'instance Oracle CDC ne s'exécute pas alors que son état est ACTIVE.|  
|d’erreur|0| 1|L'instance Oracle CDC ne s'exécute pas. L'état ERROR indique que l'instance de capture de données modifiées était ACTIVE, mais a rencontré une erreur qui n'est pas récupérable et s'est désactivée elle-même.|MISCONFIGUED : une erreur de configuration irrécupérable a été détectée.<br /><br /> PASSWORD-REQUIRED : il n'existe aucun mot de passe défini pour le concepteur de capture de données modifiées pour Oracle par Attunity ou le mot de passe configuré n'est pas valide. Cela peut être dû à une modification apportée au mot de passe de la clé asymétrique du service.|  
|RUNNING| 1|0|L'instance de capture de données modifiées s'exécute et traite les enregistrements de modification.|IDLE : tous les enregistrements de modification ont été traités et stockés dans les tables de contrôle cibles (**_CT**). Aucune transaction n'est active avec les tables de contrôle.<br /><br /> PROCESSING : il existe des enregistrements de modification en cours de traitement qui ne sont pas encore écrits dans les tables de contrôle (**_CT**).|  
|STOPPED|0|0|L'instance CDC n'est pas en cours d'exécution.|Le sous-état STOP indique que l'instance de capture de données modifiées était ACTIVE et a été arrêtée correctement.|  
|SUSPENDED| 1| 1|L'instance de capture de données modifiées s'exécute, mais le traitement est interrompu en raison d'une erreur récupérable.|DISCONNECTED : il est impossible d'établir la connexion à la base de données Oracle source. Le traitement se poursuit une fois la connexion restaurée.<br /><br /> STORAGE : le stockage est saturé. Le traitement se poursuit une fois le stockage disponible. Dans certains cas, cet état n'apparaît pas, car il est impossible de mettre à jour la table d'état.<br /><br /> LOGGER : le journal est connecté à Oracle, mais il ne peut pas lire les journaux des transactions Oracle en raison d'un problème temporaire.|  
|DATAERROR|x|x|Ce code d’état est utilisé uniquement pour la table **xdbcdc_trace** . Il n’apparaît pas dans la table **xdbcdc_state** . Les enregistrements de trace avec cet état indiquent un problème avec un enregistrement de journal Oracle. L’enregistrement de journal incorrect est stocké dans la colonne **data** en tant que BLOB.|BADRECORD : impossible d'analyser l'enregistrement du journal joint.<br /><br /> CONVERT-ERROR : impossible de convertir les données de certaines colonnes en colonnes cibles dans la table de capture. Cet état apparaît uniquement si la configuration spécifie que les erreurs de conversion doivent produire des enregistrements de trace.|  
  
 Étant donné que l'état du service de capture de données modifiées Oracle est stocké dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], il peut y avoir des cas où la valeur d'état dans la base de données ne reflète pas l'état réel du service. Le scénario le plus courant est lorsque le service perd sa connexion à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et ne peut pas la rétablir (pour une raison quelconque). Dans ce cas, l’état stocké dans **cdc.xdbcdc_state** devient obsolète. Si la valeur de l'horodateur de dernière mise à jour (UTC) date de plus d'une minute, l'état est probablement obsolète. Dans ce cas, utilisez l'Observateur d'événements Windows pour rechercher des informations supplémentaires sur l'état du service.  
  
## <a name="error-handling"></a>Gestion des erreurs  
 Cette section décrit comment le service de capture de données modifiées Oracle gère les erreurs.  
  
### <a name="logging"></a>Journalisation  
 Le service de capture de données modifiées Oracle crée des informations d'erreur dans l'un des emplacements suivants.  
  
-   Le journal des événements Windows, qui est utilisé avec la journalisation des erreurs et pour indiquer les événements de cycle de vie de service de capture de données modifiées Oracle (démarrage, arrêt, reconnexion à l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] cible).  
  
-   La table MSXDBCDC.dbo.xdbcdc_trace, utilisée pour la journalisation et le suivi généraux par le processus principal du service de capture de données modifiées Oracle.  
  
-   La table \<cdc-database>.cdc.xdbcdc_trace, utilisée pour la journalisation et le suivi généraux par les instances Oracle CDC. Cela signifie que les erreurs liées à une instance Oracle CDC spécifique sont enregistrées dans la table de trace de cette instance.  
  
 Les informations sont enregistrées par le service de capture de données modifiées Oracle lorsque le service :  
  
-   est démarré ou arrêté par le gestionnaire de contrôle des services ;  
  
-   ne peut pas se connecter à l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] associée et lorsqu'il établit avec succès une connexion après un échec ;  
  
-   rencontre une erreur lors du démarrage des instances de service de capture de données modifiées Oracle ;  
  
-   détecte qu'une instance Oracle CDC s'est interrompue ;  
  
-   rencontre une erreur inattendue.  
  
 Les informations sont enregistrées par l'instance de capture de données modifiées lorsque l'instance :  
  
-   est activée ou désactivée ;  
  
-   rencontre une erreur ;  
  
-   récupère à partir d'une erreur récupérable.  
  
 La table de trace est également utilisée pour consigner les informations de trace détaillées en vue du dépannage.  
  
### <a name="handling-source-oracle-connection-errors"></a>Gestion des erreurs de connexion Oracle à la source  
 Le service de capture de données modifiées Oracle a besoin d'une connexion permanente à la base de données Oracle source. De nombreuses erreurs de connexion qui ne sont pas liées à la configuration du service (telles que les erreurs réseau) sont considérées comme étant transitoires. Par conséquent, si le service de capture de données modifiées Oracle ne peut pas établir de connexion à la base de données Oracle (lors du démarrage ou pendant le travail suivant une déconnexion), le service change son état en SUSPENDED/DISCONNECTED et entre dans une boucle de tentatives où la connexion est retentée à intervalles réguliers. Lorsque la connexion est rétablie, le traitement continue.  
  
 D'autres types d'erreurs de connexion ne sont pas transitoires (par exemple, informations d'identification incorrectes, privilèges insuffisants et adresse de base de données incorrecte). Lorsque ces erreurs se produisent, l'état du service de capture de données modifiées Oracle est défini à ERROR/MISCONFIGURED ou à ERROR/PASSWORD-REQUIRED et le service est désactivé. Lorsque l'utilisateur corrige l'erreur sous-jacente, l'instance Oracle CDC doit être réactivée manuellement pour que le traitement continue.  
  
 Lorsqu'il n'apparaît pas clairement si l'erreur est temporaire, il est préférable de supposer qu'elle est temporaire.  
  
### <a name="handling-target-sql-server-connection-errors"></a>Gestion des erreurs de connexion à l'instance SQL Server cible  
 Le service de capture de données modifiées Oracle a besoin d'une connexion permanente à l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] cible. Cette connexion sert à :  
  
-   Vérifier qu'il n'y a aucun autre service portant le même nom actuellement utilisé avec cette instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
-   Vérifier quelle instance Oracle CDC est activée ou désactivée et démarrer (ou arrêter) son sous-processus.  
  
 Lorsque le service établit une connexion à l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] cible et vérifie qu'il s'agit du seul service de capture de données modifiées Oracle portant ce nom qui fonctionne, il peut vérifier les instances Oracle CDC qui sont activées et démarrer leurs processus de gestion (après le service arrête ces processus lorsqu'elles sont désactivées). Les instances Oracle CDC utilisent leurs connexions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour utiliser la base de données CDC de l'instance Oracle CDC.  
  
 Le mode de gestion des erreurs en cas d'échec de la connexion à l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] cible varie selon que les erreurs sont temporaires ou non.  
  
 Pour les erreurs non temporaires (telles que informations d'identification incorrectes, privilèges insuffisants, informations de connexion incorrectes), le service enregistre une erreur dans le journal des événements Windows et s'arrête (il ne peut pas écrire dans la table de trace, car il ne peut pas se connecter à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]). Dans ce cas, l'utilisateur doit résoudre l'erreur et redémarrer le service Windows de capture de données modifiées Oracle.  
  
 Pour les erreurs temporaires et les erreurs inattendues, l'opération est retentée plusieurs fois et si l'erreur persiste pendant une période significative, le service de capture de données modifiées arrête les sous-processus de l'instance CDC et revient à sa tentative de connexion initiale (à ce moment-là, un service de capture de données modifiées Oracle sur un autre ordinateur peut déjà avoir pris le contrôle du service de capture de données modifiées nommé).  
  
### <a name="handling-target-sql-server-storage-full-errors"></a>Gestion des erreurs de stockage saturé sur l'instance SQL Server cible  
 Lorsque le service de capture de données modifiées Oracle détecte qu'il ne peut pas insérer de nouvelles données dans la base de données CDC [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] cible, il écrit un avertissement dans le journal des événements et tente d'insérer un enregistrement de trace (bien que cette opération puisse échouer pour la même raison). Il effectue une nouvelle tentative dans un intervalle spécifique jusqu'à ce que l'opération réussisse.  
  
### <a name="handling-oracle-cdc-errors"></a>Gestion des erreurs Oracle CDC  
 L'instance Oracle CDC lit le journal des transactions Oracle et le traite. Si le traitement CDC rencontre une erreur, il apparaît dans la table **cdc.xdbcdc_state** et l’utilisateur doit intervenir manuellement selon l’erreur signalée.  
  
 Par exemple, l'instance Oracle CDC ne peut pas être active pour une durée étendue et les journaux des transactions Oracle requis ne sont plus disponibles. Dans ce cas, l'administrateur de base de données Oracle doit restaurer les journaux requis de façon à ce que l'instance Oracle CDC continue le traitement.  
  
### <a name="handling-unexpected-oracle-cdc-instance-failures"></a>Gestion des échecs inattendus d'instance Oracle CDC  
 Le service de capture de données modifiées Oracle surveille ses sous-processus d'instance CDC. Lorsqu'un sous-processus d'instance de capture de données modifiées s'interrompt, le service de capture de données modifiées le désactive dans la table MSXDBCDC.dbo.xdbcdc_databases et met à jour l'état cdc.xdbcdc_state vers ABORTED. Dans ce cas, la boîte de dialogue Rapport d'erreurs Windows est utilisée pour signaler cette erreur en vue d'une analyse ultérieure.  
  
## <a name="see-also"></a> Voir aussi  
 [Concepteur de capture de données modifiées pour Oracle par Attunity](../../integration-services/change-data-capture/change-data-capture-designer-for-oracle-by-attunity.md)   
 [Instance CDC Oracle](../../integration-services/change-data-capture/the-oracle-cdc-instance.md)  
  
  
