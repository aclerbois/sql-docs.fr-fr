---
title: Mise en cache des rapports | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-server
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- report execution properties [Reporting Services]
- cache [Reporting Services]
- report processing [Reporting Services], caching
- cached instances [Reporting Services]
- refreshing cache
- cached reports [Reporting Services]
- preloading cache
- invalid cached reports [Reporting Services]
- performance [Reporting Services]
- expiration [Reporting Services]
- snapshots [Reporting Services], caching
ms.assetid: 146542c3-8efd-4b89-a8d8-77d22896630e
caps.latest.revision: 44
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 9cf232e3d40a4463b9880bcefdc6bfb7f26f2358
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="caching-reports-ssrs"></a>Mise en cache de rapports (SSRS)
  Un serveur de rapports peut mettre en mémoire cache la copie d'un rapport traité et retourner cette copie lorsqu'un utilisateur ouvre le rapport. Pour cet utilisateur, la date et l'heure de l'exécution du rapport sont les seules informations qui lui permettent de savoir que ce rapport est une copie en cache. Si la date ou l'heure n'est pas celle en cours et que le rapport n'est pas un instantané, ceci signifie que le rapport a été extrait du cache.  
  
 La mise en cache peut raccourcir le temps nécessaire à la récupération d'un rapport si celui-ci est volumineux ou fréquemment consulté. Si le serveur est redémarré, toutes les instances mises en cache sont réintégrées lorsque le service Web Report Server revient en ligne.  
  
 La mise en cache est une technique d'optimisation des performances. Le contenu du cache est volatile et peut changer à mesure que les rapports sont ajoutés, remplacés ou supprimés. Si vous avez besoin d'une stratégie de mise en cache moins aléatoire, créez un instantané de rapport. Pour plus d’informations, consultez [Définir les propriétés de traitement d’un rapport](../../reporting-services/report-server/set-report-processing-properties.md).  
  
> [!NOTE]  
>  [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] stocke les fichiers temporaires dans une base de données afin de prendre en charge les sessions utilisateur et le traitement des rapports. Ces fichiers sont mis en cache pour être utilisés en interne et pour assurer un affichage constant durant une même session de navigateur. Pour plus d’informations sur la façon dont les fichiers temporaires à usage interne sont mis en cache, consultez [Base de données du serveur de rapports &#40;SSRS en mode natif&#41;](../../reporting-services/report-server/report-server-database-ssrs-native-mode.md).  
  
## <a name="cached-instances"></a>Instances mises en cache  
 L'instance mise en cache d'un rapport est basée sur le format intermédiaire d'un rapport. Le serveur de rapports met généralement en cache l'instance d'un rapport sur la base du nom du rapport. Si toutefois un rapport peut contenir des données différentes basées sur des paramètres de requête, plusieurs versions du rapport peuvent être mises en cache à un moment donné. Supposons, par exemple, que vous disposez d'un rapport paramétrable qui prend un code de région en tant que valeur de paramètre. Si quatre utilisateurs différents spécifient quatre codes de région différents, quatre copies mises en cache sont créées.  
  
 Le premier utilisateur qui exécute le rapport avec un code de région unique crée un rapport mis en cache contenant des données pour cette région. Les utilisateurs suivants qui demandent le rapport en utilisant le même code de région obtiennent la copie mise en cache.  
  
 Les rapports ne peuvent pas tous être mis en cache. Par exemple, les rapports qui contiennent des données tributaires de l'utilisateur, ceux qui nécessitent la saisie d'informations d'identification par l'utilisateur ou encore ceux qui utilisent l'authentification Windows.  
  
## <a name="refreshing-the-cache"></a>Actualisation du cache  
 La nouvelle version d'un rapport est substituée à l'exemplaire mis en cache lorsqu'un utilisateur sélectionne le rapport après l'expiration de la copie précédemment chargée dans la mémoire. Les rapports configurés pour s'exécuter en tant qu'instances en cache sont supprimés du cache à intervalles réguliers, conformément aux paramètres d'expiration définis. Vous pouvez choisir le mode d'expiration d'un rapport, en minutes ou à une heure planifiée, selon les conditions dictées par le caractère urgent des données. Vous ne pouvez pas supprimer directement un rapport du cache, à moins d'utiliser l'interface API SOAP.  
  
 Pour configurer l'expiration du cache, utilisez une planification partagée ou propre au rapport. Si vous utilisez une planification partagée qui est suspendue, le cache n'arrive pas à expiration tant que la planification n'est pas active. Si la planification partagée est supprimée, une copie des paramètres de planification est enregistrée en tant que planification propre au rapport.  
  
 Si une planification arrive à expiration ou si le moteur de planification n'est pas disponible à la date d'expiration du cache, le serveur de rapports exécute un rapport en direct jusqu'à ce que les opérations planifiées puissent reprendre (soit en étendant la planification, soit en démarrant le service de planification).  
  
## <a name="preloading-the-cache"></a>Préchargement du cache  
 Pour améliorer les performances d'un serveur, préchargez le cache. Vous pouvez précharger le cache avec une collection d'instances de rapport paramétrable de deux façons :  
  
1.  Créez un plan d'actualisation du cache. Lorsque vous créez un plan d'actualisation, vous pouvez spécifier une planification pour un rapport unique ou spécifier une planification partagée.  
  
2.  Créez un abonnement piloté par les données qui utilise le fournisseur de remise Null. Lorsque vous spécifiez le fournisseur de remise Null comme méthode de remise dans l'abonnement, le serveur de rapports cible la base de données du serveur de rapports comme destination de remise et utilise une extension de rendu particulière du nom d'extension de rendu Null. À la différence des autres extensions de remise, le fournisseur de remise Null ne propose aucun paramètre de remise configurable par le biais d'une définition d'abonnement.  
  
 La mise en cache d'un rapport est particulièrement utile si vous souhaitez mettre en cache plusieurs instances d'un rapport paramétrable dans lesquelles différentes valeurs de paramètres sont utilisées pour produire différentes instances de rapport. Notez que vous ne pouvez spécifier que des paramètres reposant sur des requêtes dans le rapport.  
  
 Lorsque vous spécifiez une planification ou créez l'abonnement piloté par les données, vous devez planifier la fréquence de remise de ces rapports dans le cache. Ces copies de rapports doivent avoir expiré pour que de nouveaux exemplaires les remplacent dans le cache. Ainsi, les propriétés d'exécution du rapport doivent être configurées pour englober les paramètres d'expiration du cache. Les valeurs de ces paramètres doivent tenir compte de la planification de l'abonnement que vous définissez. De fait, si vous créez un abonnement qui s'exécute chaque nuit, le cache doit également expirer chaque nuit, avant l'heure d'exécution de l'abonnement. Si les propriétés d'exécution n'incluent aucune limite d'expiration, les remises plus récentes sont ignorées. Pour plus d’informations sur les plans d’actualisation du cache, consultez [Planifications](../../reporting-services/subscriptions/schedules.md). Pour plus d’informations sur la définition des propriétés, consultez [Définir les propriétés de traitement d’un rapport](../../reporting-services/report-server/set-report-processing-properties.md). Pour plus d’informations sur l’utilisation des abonnements pilotés par les données, consultez [Abonnements pilotés par les données](../../reporting-services/subscriptions/data-driven-subscriptions.md).  
  
## <a name="conditions-that-cause-cache-expiration"></a>Conditions entraînant l’expiration du cache  
 Un rapport mis en cache peut être invalidé si les événements suivants se produisent : la définition du rapport est modifiée, les paramètres de rapport sont modifiés, les informations d'identification de la source de données sont modifiées ou les options d'exécution du rapport sont modifiées. Si vous supprimez un rapport stocké dans le cache, la version mise en cache disparaît également.  
  
 Si un rapport ne peut pas faire l'objet d'un rendu à partir d'une instance mise en cache pour une raison quelconque (par exemple, si les valeurs de paramètres spécifiées par un utilisateur sont différentes de celles utilisées pour produire le rapport mis en cache), le serveur de rapports réexécute le rapport.  
  
## <a name="see-also"></a> Voir aussi  
 [Définir les options de traitement &#40;Reporting Services en mode intégré SharePoint&#41;](../../reporting-services/report-server-sharepoint/set-processing-options-reporting-services-in-sharepoint-integrated-mode.md)   
 [Définir les propriétés de traitement d'un rapport](../../reporting-services/report-server/set-report-processing-properties.md)   
 [Concepts de Reporting Services &#40;SSRS&#41;](../../reporting-services/reporting-services-concepts-ssrs.md)   
 [Précharger le cache &#40;Gestionnaire de rapports&#41;](../../reporting-services/report-server/preload-the-cache-report-manager.md)   
 [Planifications](../../reporting-services/subscriptions/schedules.md)   
 [Mettre en cache les datasets partagés &#40;SSRS&#41;](../../reporting-services/report-server/cache-shared-datasets-ssrs.md)   
 [Options d’actualisation du cache &#40;Gestionnaire de rapports&#41;](http://msdn.microsoft.com/library/227da40c-6bd2-48ec-aa9c-50ce6c1ca3a6)  
  
  
