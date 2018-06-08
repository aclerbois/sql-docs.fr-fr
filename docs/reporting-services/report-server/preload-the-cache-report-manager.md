---
title: Précharger le cache (Gestionnaire de rapports) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-server
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- cache [Reporting Services]
- preloading cache
ms.assetid: 152a1051-8aa5-4c01-bc85-f8be8971b0cd
caps.latest.revision: 35
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: e1bd75c7973c603cdcd9093f07190b1f3fa70c8b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="preload-the-cache-report-manager"></a>préchargement du cache (Gestionnaire de rapports)
  Vous pouvez précharger le cache pour un dataset partagé en créant un plan d'actualisation du cache pour le dataset partagé.  
  
 Vous pouvez précharger le cache pour un rapport de deux façons :  
  
1.  Créer un plan d'actualisation du cache pour le rapport. Cette méthode est recommandée.  
  
2.  Utilisez l'abonnement piloté par les données pour précharger des instances de rapports paramétrables dans le cache. Cette méthode était la seule permettant de précharger le cache dans les versions de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] antérieures à [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]. Pour plus d’informations, consultez [Mise en cache de rapports &#40;SSRS&#41;](../../reporting-services/report-server/caching-reports-ssrs.md).  
  
 Les conditions suivantes doivent être réunies avant de pouvoir mettre en cache un rapport ou un dataset partagé :  
  
-   La mise en cache doit être activée pour le dataset partagé ou le rapport.  
  
-   Les sources de données partagées pour le dataset partagé ou le rapport doivent être configurées pour utiliser des informations d'identification stockées ou aucune information d'identification.  
  
-   Le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent doit être en cours d’exécution.  
  
### <a name="to-preload-the-cache-by-creating-a-cache-refresh-plan"></a>Pour précharger le cache en créant un plan d'actualisation du cache  
  
1.  Démarrez le [Gestionnaire de rapports &#40;SSRS en mode natif&#41;](http://msdn.microsoft.com/library/80949f9d-58f5-48e3-9342-9e9bf4e57896).  
  
2.  Dans le Gestionnaire de rapports, accédez à la page **Contenu** , puis à l’élément que vous voulez mettre en cache.  
  
3.  Pointez sur l’élément, cliquez sur la liste déroulante, puis cliquez sur **Gérer**.  
  
4.  Cliquez sur l’onglet **Options d’actualisation du cache** .  
  
5.  Dans la barre d’outils, cliquez sur **Nouveau plan d’actualisation du cache**.  
  
    > [!NOTE]  
    >  Si la mise en cache n'est pas activée pour l'élément, vous serez invité à l'activer. Pour activer la mise en cache, cliquez sur **OK**.  
  
     La page Plan d'actualisation du cache s'ouvre.  
  
6.  Tapez éventuellement une description pour le plan d'actualisation.  
  
7.  Pour une planification partagée, cliquez sur **Planification partagée**, puis sélectionnez le nom de la planification à utiliser.  
  
     Pour une planification personnalisée, cliquez sur **Planification spécifique aux éléments**, puis cliquez **Configurer**.  
  
8.  Configurer la planification  
  
9. [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
### <a name="to-preload-the-cache-with-a-user-specific-report-by-using-a-data-driven-subscription"></a>Pour précharger le cache avec un rapport spécifique à l'utilisateur en utilisant un abonnement piloté par les données  
  
1.  Démarrez le [Gestionnaire de rapports &#40;SSRS en mode natif&#41;](http://msdn.microsoft.com/library/80949f9d-58f5-48e3-9342-9e9bf4e57896).  
  
2.  Dans le Gestionnaire de rapports, accédez à la page **Contenu** , puis au rapport pour lequel vous voulez créer un abonnement.  
  
3.  Cliquez successivement sur le rapport, sur l’onglet **Abonnements** , puis sur **Nouvel abonnement piloté par les données**.  
  
4.  Tapez éventuellement une description pour l'abonnement.  
  
5.  Dans la liste **Spécifiez le mode de notification des destinataires** , sélectionnez **Fournisseur de remise Null**.  
  
6.  Spécifiez un type de source de données, puis cliquez sur **Suivant** pour configurer la source de données.  
  
7.  Spécifiez le type de connexion, la chaîne de connexion et les informations d'identification pour accéder à la source de données contenant les données des abonnés. L'exemple suivant illustre une chaîne de connexion à la base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] appelée Subscribers :  
  
    ```  
    data source=<servername>; initial catalog=Subscribers  
    ```  
  
8.  Cliquez sur **Suivant**.  
  
9. Spécifiez la requête ou la commande qui permet de récupérer les données des abonnés. Augmentez éventuellement le délai d'attente des requêtes dont le traitement est long. Exemple :  
  
    ```  
    Select * from UserInfo  
    ```  
  
10. Cliquez sur **Valider**. La requête doit être validée pour pouvoir continuer. Quand le message **Réussite de la validation de la requête** s’affiche, cliquez sur **Suivant**.  
  
11. Les paramètres d’extension de remise n’étant pas configurables pour le fournisseur de remise Null, cliquez sur **Suivant**.  
  
12. Spécifiez les valeurs des paramètres du rapport pour l’abonnement, puis cliquez sur **Suivant**.  
  
13. Précisez le moment où l'abonnement doit être traité. Ne choisissez pas l’option **Lorsque les données du rapport sont mises à jour sur le serveur de rapports**. Elle est réservée exclusivement aux instantanés. Si vous voulez utiliser une planification préexistante, sélectionnez **Suivant une planification partagée**.  
  
     Ou, pour créer une planification personnalisée, cliquez sur **Suivant une planification créée pour cet abonnement** , puis sur **Suivant**. Configurez la planification, puis cliquez sur **Terminer**.  
  
    > [!NOTE]  
    >  Pour que les abonnés reçoivent la version du rapport la plus récente, la planification que vous configurez doit être chronologiquement cohérente par rapport à la planification de la remise du rapport que vous avez définie pour les abonnés. Pour plus d’informations, consultez [Gestionnaire de rapports &#40;SSRS en mode natif&#41;](http://msdn.microsoft.com/library/80949f9d-58f5-48e3-9342-9e9bf4e57896).  
  
14. Configurez les options d'exécution du rapport comme suit. Dans la page du rapport, cliquez sur l’onglet **Propriétés** .  
  
15. Dans le cadre de gauche, cliquez sur l’onglet **Exécution** .  
  
16. Dans la page qui s’affiche, sélectionnez **Effectuer le rendu de ce rapport avec les données les plus récentes**.  
  
17. Choisissez l'une des deux options de mise en cache suivantes et configurez l'expiration comme suit :  
  
    -   Pour effectuer l'expiration d'une copie mise en cache après l'écoulement d'une durée particulière, cliquez sur **Mettre en cache une copie temporaire du rapport. Faire expirer la copie du rapport après un certain nombre de minutes.** Tapez le nombre de minutes pour l'expiration du rapport.  
  
    -   Pour définir l’expiration d’une copie mise en cache selon une planification, cliquez sur **Mettre en cache une copie temporaire du rapport. Faire expirer la copie du rapport selon la planification suivante.** Cliquez sur **Configurer**ou sélectionnez une planification partagée pour définir l’expiration du rapport.  
  
18. Cliquez sur **Appliquer**.  
  
## <a name="see-also"></a> Voir aussi  
 [Abonnements pilotés par les données](../../reporting-services/subscriptions/data-driven-subscriptions.md)   
 [Créer un abonnement piloté par les données &#40;didacticiel SSRS&#41;](../../reporting-services/create-a-data-driven-subscription-ssrs-tutorial.md)   
 [Performances, instantanés, mise en cache &#40;Reporting Services&#41;](../../reporting-services/report-server/performance-snapshots-caching-reporting-services.md)   
 [Définir les propriétés de traitement d'un rapport](../../reporting-services/report-server/set-report-processing-properties.md)   
 [Mise en cache de rapports &#40;SSRS&#41;](../../reporting-services/report-server/caching-reports-ssrs.md)  
  
  
