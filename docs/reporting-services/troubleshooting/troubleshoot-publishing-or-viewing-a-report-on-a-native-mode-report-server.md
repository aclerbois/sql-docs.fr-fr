---
title: Résoudre les problèmes liés à la publication ou à l’affichage d’un rapport sur un serveur de rapports en mode natif | Microsoft Docs
ms.custom: ''
ms.date: 02/28/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: troubleshooting
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: df7720a1-d178-45bb-8d6f-63e208cae7fe
caps.latest.revision: 6
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: de0d300991457ce6ba07a4f499f31224b6d38fbb
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="troubleshoot-publishing-or-viewing-a-report-on-a-native-mode-report-server"></a>Résolution des problèmes liés à la publication ou à l’affichage d’un rapport sur un serveur de rapports en mode natif
  
  
  
Lorsque vous publiez ou téléchargez un rapport sur un serveur de rapports configuré en mode natif, des problèmes spécifiques à l'affichage de rapports sur le serveur de rapports peuvent se produire. Utilisez cette rubrique pour vous aider à résoudre ces problèmes.   
  
## <a name="why-am-i-being-prompted-for-credentials-when-i-publish-a-report"></a>Pourquoi ne suis-je pas invité à fournir des informations d'identification lorsque je publie un rapport ?  
Pour déployer un rapport dans un serveur de rapports, vous devez spécifier l'adresse du serveur. La boîte de dialogue Ouverture de session Reporting Services peut s'afficher pour vous inviter à entrer des informations d'identification.   
  
Le nom du serveur de rapports n'est pas spécifié correctement  
  
  
Lors du déploiement du rapport dans un serveur de rapports en mode natif, il arrive fréquemment de spécifier le nom du dossier de rapports au lieu du nom du serveur de rapports.   
  
Vérifiez que l’URL du serveur de rapports correspond à l’adresse du serveur de rapports (par exemple, `http://localhost/reportserver`), et non à l’adresse du répertoire virtuel Gestionnaire de rapports (par exemple, `http://localhost/reports`). Si vous avez spécifié un numéro de port pour le serveur de rapports qui est différent du numéro de port par défaut (80), vous devez le spécifier dans l’adresse du serveur de rapports (par exemple, `http://localhost:81/reportserver`).   
  
 ## <a name="nothing-happens-when-i-toggle-items-in-my-published-report"></a>Rien ne se produit lorsque je bascule des éléments dans mon rapport publié.  
  Lorsque vous affichez un rapport dans l'aperçu local, vous pouvez basculer des éléments dans le rapport et les afficher ou les masquer. Lorsque vous affichez le même rapport une fois que celui-ci a été publié sur le serveur de rapports, basculez les éléments qui ne fonctionnent plus.   
  
\<nom du serveur de rapports> inclut un trait de soulignement (_)  
  
Si un rapport s'exécute sans erreurs, mais que les éléments de bascule ne fonctionnent pas (par exemple, rien ne se produit lorsque vous cliquez sur une icône (+) de développement), vérifiez le nom de l'ordinateur qui héberge le serveur de rapports. Si le nom de l'ordinateur contient un caractère de soulignement, les éléments de bascule ne fonctionnent pas. Il s'agit d'un problème connu. Il n’existe aucune possibilité pour contourner ce problème.   
  
Pour exécuter des rapports avec des éléments de bascule, vous devez utiliser un ordinateur dont le nom ne contient pas de caractère de soulignement.  
  
## <a name="images-and-charts-do-not-load-when-i-use-run-as-and-a-browser-to-run-my-report"></a>Les images et les graphiques ne se chargent pas lorsque j'utilise Exécuter en tant que et un navigateur pour exécuter mon rapport.  
Lorsque vous exécutez le Gestionnaire de rapports dans un contexte de sécurité différent, il est possible que tous les éléments de rapport d'un rapport ne s'affichent pas.   
  
### <a name="insufficient-permissions-on-internet-temporary-file-folders"></a>Autorisations insuffisantes sur des dossiers de fichiers temporaires Internet  
  
Dans certaines circonstances, lorsque vous utilisez le Gestionnaire de rapports pour afficher des rapports publiés qui comprennent des graphiques ou des images, il est possible qu'ils ne soient pas visibles. Par exemple, lorsque vous utilisez la commande Microsoft Windows **Run As** pour afficher un rapport dans un contexte de sécurité différent, il se peut que vous n’ayez pas les autorisations nécessaires sur le dossier dans lequel le serveur de rapports met en cache des graphiques et des images comme fichiers Internet temporaires.   
  
Vérifiez que vous êtes autorisé à accéder aux dossiers qui contiennent les fichiers mis en cache.   
    
## <a name="see-also"></a> Voir aussi  
[Planification de la prise en charge des navigateurs pour Reporting Services et Power View (Reporting Services 2014)](../../reporting-services/browser-support-for-reporting-services-and-power-view.md)  
[Erreurs et événements (Reporting Services)](../../reporting-services/troubleshooting/errors-and-events-reference-reporting-services.md)  
[Résoudre les problèmes de récupération des données avec des rapports Reporting Services](../../reporting-services/troubleshooting/troubleshoot-data-retrieval-issues-with-reporting-services-reports.md)  
[Résolution des problèmes d’abonnements et de remise de Reporting Services](../../reporting-services/troubleshooting/troubleshoot-reporting-services-subscriptions-and-delivery.md)  
  
  

[!INCLUDE[feedback_stackoverflow_msdn_connect](../../includes/feedback-stackoverflow-msdn-connect.md)]

