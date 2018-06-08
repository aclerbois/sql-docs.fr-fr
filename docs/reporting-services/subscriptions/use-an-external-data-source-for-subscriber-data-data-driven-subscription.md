---
title: Utiliser une source de données externe pour les données des abonnés (abonnements pilotés par les données) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: subscriptions
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- subscriber data sources [Reporting Services]
- subscriptions [Reporting Services], external data sources
- query-based subscriptions [Reporting Services]
- external data sources [Reporting Services]
- data-driven subscriptions
- data sources [Reporting Services], subscriptions
ms.assetid: 1cade8ec-729c-4df8-a428-e75c9ad86369
caps.latest.revision: 43
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 92e18aba9bf76129fe44bebcf5beddd4b815d0b0
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="use-an-external-data-source-for-subscriber-data-data-driven-subscription"></a>Utiliser une source de données externe pour les données des abonnés (abonnements pilotés par les données)
  Dans un abonnement piloté par les données, les données d'abonnement dynamiques sont fournies par une requête ou une commande qui récupère les données à partir d'une source de données externe. Il est possible de récupérer les données d'abonnement à partir de n'importe quelle source de données gérée qui répond aux impératifs du traitement des abonnements pilotés par les données. La syntaxe de la requête ou de la commande doit être valide pour l'extension de traitement de données installée avec votre serveur de rapports.  
  
## <a name="data-processing-requirements"></a>Impératifs liés au traitement des données  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] utilise les extensions pour le traitement des données afin de récupérer les données d’abonnement. Les types de sources de données recommandés sont les suivants :  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] bases de données relationnelles  
  
-   Bases de données Oracle  
  
-   [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] sources de données d’exploration de données et multidimensionnelles  
  
-   Sources de données XML  
  
     Lorsque vous utilisez l'extension de traitement des données XML pour les données des abonnés, veillez à augmenter les valeurs des paramètres de délai d'attente des requêtes dans l'abonnement. L'extension de traitement des données XML utilise des millisecondes (et non des secondes) pour les valeurs de délai d'attente des requêtes. Si vous n'augmentez pas la valeur du délai d'attente, il est possible que l'abonnement échoue en raison d'une durée de traitement insuffisante.  
  
     Évitez d’utiliser l’option **Informations d’identification non requises** quand vous configurez la connexion à la source de données de l’abonné. Il est recommandé d'opter pour des informations d'identification stockées lorsque vous utilisez l'extension de traitement des données XML pour récupérer les données d'abonnement lors de l'exécution.  
  
 Vous serez peut-être en mesure d'utiliser d'autres types de données pris en charge, mais il n'est pas certain qu'ils fonctionnent tous. Par exemple, les types de sources de données suivants ne peuvent pas être utilisés pour les données des abonnés :  
  
-   Bases de données SAP Netweaver BI  
  
-   Modèles de rapport  
  
 Si vous disposez d’une extension de traitement de données que vous voulez utiliser dans les abonnements pilotés par les données, cette extension doit implémenter les interfaces <xref:Microsoft.ReportingServices.DataProcessing.IDbCommand> et <xref:Microsoft.ReportingServices.DataProcessing.IDataReader> . L'extension de traitement des données doit prendre en charge une exécution de requête schéma exclusivement. Cette requête permet de récupérer les métadonnées des colonnes lors de la conception, afin que les utilisateurs puissent mapper les colonnes aux options de remise et aux paramètres de rapport dans la définition d'abonnement. L'exécution de requêtes schéma exclusivement intervient très tôt dans le processus, lorsque l'utilisateur définit l'abonnement.  
  
## <a name="query-requirements"></a>Impératifs liés aux requêtes  
 Lorsque vous créez une requête qui récupère les données d'abonnement, gardez à l'esprit les points suivants :  
  
-   Vous ne pouvez créer qu'une seule requête pour l'abonnement.  
  
-   La requête doit retourner toutes les valeurs que vous voulez utiliser pour les options de remise et pour la spécification des paramètres de rapport.  
  
-   Le serveur de rapports créera une remise de rapport pour chaque ligne du jeu de résultats. Si le premier jeu de résultats est composé de trois cents lignes, le serveur de rapports tentera de remettre trois cents rapports.  
  
## <a name="setting-delivery-options-using-variable-data-from-a-subscriber-database"></a>Définition des options de remise à l'aide de données de variable d'une base de données d'abonnés  
 Vous pouvez utiliser les données de la base de données d'abonnés pour personnaliser les options de remise de chaque destinataire. Le type d'extension de remise que vous utilisez détermine les options disponibles. Si vous utilisez l'extension de remise par messagerie électronique du serveur de rapports, la requête doit contenir une adresse de messagerie pour chaque abonné. Si vous utilisez une extension de remise par partage de fichiers, les données d'abonnés doivent inclure des valeurs pouvant être utilisées pour créer des fichiers de rapports spécifiques aux abonnés ou pour fournir une destination pour la remise. Pour plus d’informations, consultez [Remise par courrier électronique dans Reporting Services](../../reporting-services/subscriptions/e-mail-delivery-in-reporting-services.md).  
  
## <a name="passing-parameter-values-from-the-subscriber-database-to-the-report"></a>Transmission de valeurs de paramètres de la base de données d'abonnés au rapport  
 Si vous créez un abonnement piloté par les données pour un rapport paramétrable, vous pouvez utiliser des valeurs de paramètres de variable pour personnaliser la production de chaque rapport. Par exemple, une base de données d'abonnés peut contenir des numéros d'identification d'employés, des dates d'embauche, des postes et des adresses de lieux de travail dont vous pouvez vous servir pour filtrer les données du rapport. Si le rapport accepte des paramètres basés sur ces données de colonnes, vous pouvez mapper le paramètre à la colonne appropriée.  
  
 Lors du mappage de champs d'abonné à des paramètres de rapport, vérifiez que les types de données et les longueurs de colonnes sont compatibles. En cas de non-correspondance du type de données, une erreur se produit lors du traitement des abonnements. Pour en savoir plus sur l’utilisation des données dans un rapport paramétrable, consultez [Créer un abonnement piloté par les données &#40;didacticiel SSRS&#41;](../../reporting-services/create-a-data-driven-subscription-ssrs-tutorial.md).  
  
## <a name="modifying-the-subscriber-data-source"></a>Modification de la source de données des abonnés  
 Les modifications suivantes apportées à la source de données peuvent empêcher l'exécution de l'abonnement :  
  
-   suppression des colonnes référencées dans l'abonnement ;  
  
-   modification de la structure de la table de la source de données ;  
  
-   modification du type de données et de diverses propriétés de colonne.  
  
 Si vous procédez à des modifications de ce type, vous devez mettre à jour l'abonnement.  
  
## <a name="see-also"></a> Voir aussi  
 [Créer, modifier ou supprimer des abonnements pilotés par les données](../../reporting-services/subscriptions/create-modify-and-delete-data-driven-subscriptions.md)   
 [Abonnements pilotés par les données](../../reporting-services/subscriptions/data-driven-subscriptions.md)   
 [Abonnements et remise &#40;Reporting Services&#41;](../../reporting-services/subscriptions/subscriptions-and-delivery-reporting-services.md)  
  
  
