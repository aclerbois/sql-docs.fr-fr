---
title: Définir des seuils et des avertissements dans le Moniteur de réplication | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: replication
ms.reviewer: ''
ms.suite: sql
ms.technology:
- replication
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- alerts [SQL Server replication]
- Merge Agent, thresholds and warnings
- Distribution Agent, thresholds and warnings
- thresholds [SQL Server replication]
- Replication Monitor, thresholds and warnings
- monitoring performance [SQL Server replication], thresholds and warnings
ms.assetid: 3a409c2c-b77e-4001-b81a-1dcd918618ec
caps.latest.revision: 33
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 4cd9334c240c2206f7e226b1c63c36d53cdeeed1
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="set-thresholds-and-warnings-in-replication-monitor"></a>Définir des seuils et des avertissements dans le Moniteur de réplication
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Le moniteur de réplication[!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] présente des informations sur l'état des publications et des abonnements. Par défaut, le Moniteur de réplication affiche des avertissements uniquement pour les abonnements non initialisés, mais vous pouvez activer les avertissements pour d'autres conditions. Il est recommandé d'activer les avertissements pour votre topologie, afin que vous soyez informés de l'état et des performances en temps voulu.  
  
 Quand vous activez un avertissement, vous spécifiez un seuil. Lorsque ce seuil est atteint ou dépassé, un avertissement est affiché (à moins qu'un problème plus important ne doive être affiché). L'atteinte d'un seuil déclenche un avertissement dans le Moniteur de réplication, mais également une alerte. Vous pouvez activer des avertissements pour les situations suivantes :  
  
-   Expiration imminente d'un abonnement  
  
     Ceci concerne tous les types de réplications. Si le seuil spécifié est atteint ou dépassé, l'état de l'abonnement s'affiche sous la forme **Expire bientôt/Expiré**.  
  
-   Dépassement de la latence spécifiée (la quantité de temps qui s'écoule entre la validation d'une transaction sur le serveur de publication et la validation de la transaction correspondante sur l'Abonné).  
  
     Ceci s'applique à la réplication transactionnelle. Si le seuil spécifié est atteint ou dépassé, l'état de l'abonnement s'affiche sous la forme **Critique pour les performances**.  
  
-   Dépassement du temps de synchronisation spécifié.  
  
     Ceci s'applique à la réplication de fusion. Si le seuil spécifié est atteint ou dépassé, l'état affiché est **Fusion longue**. Vous pouvez spécifier des seuils différents pour des connexions à distance et des connexions sur réseau local.  
  
-   Impossibilité de traiter le nombre de lignes spécifié dans un intervalle de temps donné.  
  
     Ceci s'applique à la réplication de fusion. Si le seuil spécifié est atteint ou dépassé, l'état affiché est **Critique pour les performances**. Vous pouvez spécifier des seuils différents pour des connexions à distance et des connexions sur réseau local.  
  
 Pour plus d’informations sur les avertissements de type **Critique pour les performances** et **Fusion longue**, consultez [Analyser les performances avec le Moniteur de réplication](../../../relational-databases/replication/monitor/monitor-performance-with-replication-monitor.md).  
  
 **Dans cette rubrique**  
  
-   [Définir des seuils et des avertissements pour une publication transactionnelle](#Transactional)  
  
-   [Définir des seuils et des avertissements pour une publication de fusion](#Merge)  
  
-   [Définir des seuils et des avertissements pour une publication d'instantané](#Snapshot)  
  
##  <a name="Transactional"></a> Pour définir des seuils et des avertissements pour une publication transactionnelle  
  
1.  Développez un groupe de serveurs de publication dans le volet gauche, développez un serveur de publication, puis cliquez sur une publication.  
  
2.  Cliquez sur l'onglet **Avertissements** . Pour afficher des informations plus complètes sur les options de cet onglet, cliquez sur **Aide** dans la barre de menus.  
  
3.  Sélectionnez un avertissement en activant la case à cocher appropriée : **Avertir si un abonnement expire avant le seuil défini** ou **Avertir si la latence dépasse le seuil**.  
  
4.  Définissez un seuil pour les avertissements dans la colonne **Seuil** . Si, par exemple, vous avez sélectionné **Avertir si la latence dépasse le seuil** à l'étape 3, vous pouvez choisir une latence de **60 secondes** dans la colonne **Seuil** .  
  
5.  Cliquez sur **Enregistrer les modifications**.  
  
#### <a name="to-configure-an-alert-for-a-threshold"></a>Pour configurer une alerte pour un seuil  
  
1.  Cliquez sur **Configurer des alertes**.  
  
2.  Dans la boîte de dialogue **Configurer des alertes de réplication** , sélectionnez une alerte puis cliquez sur **Configurer**.  
  
     Cette boîte de dialogue affiche des alertes pour tous les types de publications, y compris des alertes qui ne sont pas liées à la surveillance des seuils. Pour plus d’informations, consultez [Utiliser les alertes pour les événements des agents de réplication](../../../relational-databases/replication/agents/use-alerts-for-replication-agent-events.md).  
  
3.  Définissez des options dans la boîte de dialogue **Propriétés de l’alerte \<nom_alerte>**  :  
  
    -   Sur la page **Général** , cliquez sur **Activer**; spécifiez la base de données à laquelle l'alerte doit s'appliquer.  
  
    -   Sur la page **Réponse** , spécifiez si un courrier électronique doit être envoyé et/ou si un travail doit être effectué.  
  
    -   Sur la page **Options** , personnalisez le texte de la réponse.  
  
4.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
5.  Cliquez sur **Fermer**.  
  
##  <a name="Merge"></a> Définir des seuils et des avertissements pour une publication de fusion  
  
1.  Développez un groupe de serveurs de publication dans le volet gauche, développez un serveur de publication, puis cliquez sur une publication.  
  
2.  Cliquez sur l'onglet **Avertissements** . Pour afficher plus d'informations sur les options de cet onglet, sélectionnez **Aide** sur la barre de menus.  
  
3.  Sélectionnez un avertissement en activant la case à cocher appropriée :  
  
    -   **Avertir si un abonnement expire avant le seuil défini**  
  
    -   **Avertir si la durée de la fusion pour les connexions d'accès à distance dépasse le seuil**  
  
    -   **Avertir si la durée de la fusion pour les connexions LAN dépasse le seuil**  
  
    -   **Avertir si les lignes fusionnées par seconde pour les connexions LAN sont inférieures au seuil**  
  
    -   **Avertir si les lignes fusionnées par seconde pour les connexions d'accès à distance sont inférieures au seuil**  
  
4.  Définissez les seuils pour les avertissements dans la colonne **Seuil** . Par exemple, si vous avez sélectionné **Avertir si la durée de la fusion pour les connexions d'accès à distance dépasse le seuil** à l'étape 3, vous pouvez sélectionner une durée de **10 minutes** dans la colonne **Seuil** .  
  
5.  Cliquez sur **Enregistrer les modifications**.  
  
#### <a name="to-configure-an-alert-for-a-threshold"></a>Pour configurer une alerte pour un seuil  
  
1.  Cliquez sur **Configurer des alertes**.  
  
2.  Dans la boîte de dialogue **Configurer des alertes de réplication** , sélectionnez une alerte puis cliquez sur **Configurer**.  
  
     Cette boîte de dialogue affiche des alertes pour tous les types de publications, y compris des alertes qui ne sont pas liées à la surveillance des seuils.  
  
3.  Définissez les options dans la boîte de dialogue **Propriétés de l’alerte \<nom_alerte>**  :  
  
    -   Sur la page **Général** , cliquez sur **Activer**; spécifiez la base de données à laquelle l'alerte doit s'appliquer.  
  
    -   Sur la page **Réponse** , spécifiez si un courrier électronique doit être envoyé et/ou si un travail doit être effectué.  
  
    -   Sur la page **Options** , personnalisez le texte de la réponse.  
  
4.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
5.  Cliquez sur **Fermer**.  
  
##  <a name="Snapshot"></a> Définir des seuils et des avertissements pour une publication d'instantané  
  
1.  Développez un groupe de serveurs de publication dans le volet gauche, développez un serveur de publication, puis cliquez sur une publication.  
  
2.  Cliquez sur l'onglet **Avertissements** . Pour afficher plus d'informations sur les options de cet onglet, cliquez sur **Aide** dans le menu du haut.  
  
3.  Activez un avertissement en sélectionnant la case à cocher **Avertir si un abonnement expire avant le seuil défini**.  
  
4.  Définissez un seuil pour l'avertissement dans la colonne **Seuil** . Par exemple, vous pouvez sélectionner une valeur de **70%** dans la colonne **Seuil** .  
  
5.  Cliquez sur **Enregistrer les modifications**.  
  
#### <a name="to-configure-an-alert-for-a-threshold"></a>Pour configurer une alerte pour un seuil  
  
1.  Cliquez sur **Configurer des alertes**.  
  
2.  Dans la boîte de dialogue **Configurer des alertes de réplication** , sélectionnez une alerte puis cliquez sur **Configurer**.  
  
     Cette boîte de dialogue affiche des alertes pour tous les types de publications, y compris des alertes qui ne sont pas liées à la surveillance des seuils. Pour plus d’informations, consultez [Utiliser les alertes pour les événements des agents de réplication](../../../relational-databases/replication/agents/use-alerts-for-replication-agent-events.md).  
  
3.  Définissez des options dans la boîte de dialogue **Propriétés de l’alerte \<nom_alerte>**  :  
  
    -   Sur la page **Général** , cliquez sur **Activer**; spécifiez la base de données à laquelle l'alerte doit s'appliquer.  
  
    -   Sur la page **Réponse** , spécifiez si un courrier électronique doit être envoyé et/ou si un travail doit être effectué.  
  
    -   Sur la page **Options** , personnalisez le texte de la réponse.  
  
4.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
5.  Cliquez sur **Fermer**.  
  
## <a name="see-also"></a> Voir aussi  
 [Surveillance de la réplication](../../../relational-databases/replication/monitor/monitoring-replication-overview.md)  
  
  
