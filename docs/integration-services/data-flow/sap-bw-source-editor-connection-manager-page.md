---
title: Éditeur de source SAP BW (page Gestionnaire de connexions) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: data-flow
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.sapbwsource.connection.f1
ms.assetid: 2a6dc531-85ca-43c5-a65f-3ad3f7d537c4
caps.latest.revision: 11
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: e43d77d15fb417d201e07d389d00df0be7aad145
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sap-bw-source-editor-connection-manager-page"></a>Éditeur de source SAP BW (page Gestionnaire de connexions)
  Utilisez la page **Gestionnaire de connexions** de **l’Éditeur de source SAP BW** pour sélectionner le gestionnaire de connexions SAP BW pour la source SAP BW. Sur cette page, sélectionnez également le mode d'exécution et les paramètres pour extraire les données du système SAP Netweaver BW.  
  
 Pour en savoir plus sur le composant source SAP BW de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Connector 1.1 pour SAP BW, consultez [SAP BW Source](../../integration-services/data-flow/sap-bw-source.md).  
  
> [!IMPORTANT]  
>  La documentation de Microsoft Connector 1.1 pour SAP BW suppose que vous êtes familiarisé avec l'environnement SAP Netweaver BW. Pour plus d'informations sur SAP Netweaver BW, ou sur la configuration des objets et des processus SAP Netweaver BW objets, consultez la documentation SAP.  
  
> [!IMPORTANT]  
>  Vous devez disposer d'une licence SAP supplémentaire pour extraire des données à partir de SAP Netweaver BW. Vérifiez auprès de SAP.  
  
 **Pour ouvrir la page Gestionnaire de connexions**  
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], ouvrez le package [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] qui contient la source SAP BW.  
  
2.  Sous l’onglet **Flux de données** , double-cliquez sur la source SAP BW.  
  
3.  Dans l' **Éditeur de source SAP BW**, cliquez sur **Gestionnaire de connexions** pour ouvrir la page **Gestionnaire de connexions** de l'éditeur.  
  
## <a name="static-options"></a>Options statiques  
  
> [!NOTE]  
>  Si vous ne connaissez pas toutes les valeurs requises pour configurer la source, adressez-vous à votre administrateur SAP.  
  
 **Gestionnaire de connexions SAP BW**  
 Sélectionnez un gestionnaire de connexions existant dans la liste ou créez une nouvelle connexion en cliquant sur **Nouveau**.  
  
 **Nouveau**  
 Créez un gestionnaire de connexions à l’aide de la boîte de dialogue **Gestionnaire de connexions SAP BW** .  
  
 Pour plus d’informations sur cette boîte de dialogue, consultez [Éditeur du Gestionnaire de connexions SAP BW](../../integration-services/connection-manager/sap-bw-connection-manager-editor.md).  
  
 **Destination OHS**  
 Sélectionnez la destination OHS (Open Hub Service) à utiliser pour extraire des données de la source.  
  
 **Mode d'exécution**  
 Spécifiez la méthode d'extraction des données de la source.  
  
|Option|Description|  
|------------|-----------------|  
|**P - Déclencher une chaîne de processus**|Déclenchez une chaîne de processus. Dans ce cas, le package [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] démarre le processus d'extraction.|  
|**A - Attendre la notification**|Attendez la notification du système SAP Netweaver BW pour démarrer l'extraction des données. Dans ce cas, le système SAP Netweaver BW démarre le processus d'extraction.|  
|**E - Extraire uniquement**|Récupérez les données associées à un ID de demande particulier. Dans ce cas, le système SAP Netweaver BW a déjà extrait les données dans une table interne et le package [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] se contente de lire les données.|  
  
 **Aperçu**  
 Ouvrez la boîte de dialogue **Aperçu** dans laquelle vous pouvez afficher un aperçu des résultats. Pour plus d’informations, consultez [Aperçu](../../integration-services/data-flow/preview.md).  
  
> [!IMPORTANT]  
>  L’option **Aperçu** , disponible dans page **Gestionnaire de connexions** de l’Éditeur de source SAP BW, extrait réellement des données. Si vous avez configuré le système SAP Netweaver BW pour extraire uniquement les données qui ont été modifiées depuis l'extraction précédente, la sélection d' **Aperçu** exclura les données de l'aperçu de l'extraction suivante.  
  
 Quand vous cliquez sur **Aperçu**, vous ouvrez également la boîte de dialogue **Journal des requêtes** . Vous pouvez utiliser cette boîte de dialogue pour afficher les événements qui sont journalisés lors de la demande qui est effectuée au système SAP Netweaver BW pour les exemples de données. Pour plus d’informations, consultez [Journal des requêtes](../../integration-services/data-flow/request-log.md).  
  
## <a name="execution-mode-dynamic-options"></a>Options dynamiques du mode d'exécution  
  
> [!NOTE]  
>  Si vous ne connaissez pas toutes les valeurs requises pour configurer la source, adressez-vous à votre administrateur SAP.  
  
### <a name="execution-mode--p---trigger-process-chain"></a>Mode d'exécution = P - Déclencher une chaîne de processus  
  
#### <a name="rfc-destination-options"></a>Options de la destination RFC  
 Vous n'avez pas à connaître ni à entrer ces valeurs à l'avance. Utilisez le bouton **Rechercher** pour rechercher et sélectionner la destination RFC appropriée. Après avoir sélectionné une destination RFC, le composant entre les valeurs appropriées pour ces options.  
  
 **Hôte de passerelle**  
 Entrez le nom du serveur ou l'adresse IP de l'hôte de passerelle. Généralement, le nom ou l'adresse IP est identique au serveur d'applications SAP.  
  
 **Service de passerelle**  
 Entrez le nom du service de passerelle, au format **sapgwNN**, où **NN** représente le numéro du système.  
  
 **ID de programme**  
 Entrez l'ID du programme qui est associé à la destination RFC.  
  
 **Rechercher**  
 Recherchez la destination RFC à l’aide de la boîte de dialogue **Rechercher la destination RFC** . Pour plus d'informations sur cette boîte de dialogue, consultez [Look Up RFC Destination](../../integration-services/data-flow/look-up-rfc-destination.md).  
  
#### <a name="process-chain-options"></a>Options de chaîne de processus  
 Vous n'avez pas à connaître ni à entrer ces valeurs à l'avance. Utilisez le bouton **Rechercher** pour rechercher et sélectionner la chaîne de processus appropriée. Après avoir sélectionné une chaîne de processus, le composant entre la valeur appropriée pour l'option.  
  
 **Chaîne de processus**  
 Entrez le nom de la chaîne de processus à déclencher par la source.  
  
 **Rechercher**  
 Recherchez la chaîne de processus à l’aide de la boîte de dialogue **Rechercher la chaîne de processus** . Pour plus d’informations sur cette boîte de dialogue, consultez [Rechercher la chaîne de processus](../../integration-services/data-flow/look-up-process-chain.md).  
  
### <a name="execution-mode--w---wait-for-notify"></a>Mode d'exécution = A - Attendre la notification  
  
#### <a name="rfc-destination-options"></a>Options de la destination RFC  
 Vous n'avez pas à connaître ni à entrer ces valeurs à l'avance. Utilisez le bouton **Rechercher** pour rechercher et sélectionner la destination RFC appropriée. Une fois que vous avez sélectionné une destination RFC, le composant entre les valeurs appropriées pour les options.  
  
 **Hôte de passerelle**  
 Entrez le nom du serveur ou l'adresse IP de l'hôte de passerelle. Généralement, le nom ou l'adresse IP est identique au serveur d'applications SAP.  
  
 **Service de passerelle**  
 Entrez le nom du service de passerelle, au format **sapgwNN**, où **NN** représente le numéro du système.  
  
 **ID de programme**  
 Entrez l'ID du programme qui est associé à la destination RFC.  
  
 **Rechercher**  
 Recherchez la destination RFC à l’aide de la boîte de dialogue **Rechercher la destination RFC** . Pour plus d'informations sur cette boîte de dialogue, consultez [Look Up RFC Destination](../../integration-services/data-flow/look-up-rfc-destination.md).  
  
### <a name="execution-mode--e---extract-only"></a>Mode d'exécution - E = Extraire uniquement  
 **ID de demande**  
 Entrez l'ID de demande qui est associé à l'extraction.  
  
## <a name="see-also"></a> Voir aussi  
 [Éditeur de source SAP BW &#40;page Colonnes&#41;](../../integration-services/data-flow/sap-bw-source-editor-columns-page.md)   
 [Éditeur de source SAP BW &#40;page Sortie d’erreur&#41;](../../integration-services/data-flow/sap-bw-source-editor-error-output-page.md)   
 [Éditeur de source SAP BW &#40;page Avancé&#41;](../../integration-services/data-flow/sap-bw-source-editor-advanced-page.md)   
 [Aide (F1) sur Microsoft Connector pour SAP BW](../../integration-services/microsoft-connector-for-sap-bw-f1-help.md)  
  
  
