---
title: Éditeur de destination SAP BW (page Gestionnaire de connexions) | Microsoft Docs
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
- sql13.dts.designer.sapbwdestination.connection.f1
ms.assetid: 04ae38f8-5287-45a3-826a-8aac5dd15a91
caps.latest.revision: 10
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 8fc3eb789b9f69196b2bb963ec714d5b94ec4f20
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sap-bw-destination-editor-connection-manager-page"></a>Éditeur de destination SAP BW (page Gestionnaire de connexions)
  Utilisez la page **Gestionnaire de connexions** de l' **Éditeur de destination SAP BW** pour sélectionner le gestionnaire de connexions SAP BW qui sera utilisé par la destination SAP BW. Sur cette page, sélectionnez également les paramètres pour charger les données dans le système SAP Netweaver BW.  
  
 Pour en savoir plus sur la destination SAP BW de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Connector 1.1 pour SAP BW, consultez [Destination SAP BW](../../integration-services/data-flow/sap-bw-destination.md).  
  
> [!IMPORTANT]  
>  La documentation de Microsoft Connector 1.1 pour SAP BW suppose que vous êtes familiarisé avec l'environnement SAP Netweaver BW. Pour plus d'informations sur SAP Netweaver BW, ou sur la configuration des objets et des processus SAP Netweaver BW objets, consultez la documentation SAP.  
  
 **Pour ouvrir la page Gestionnaire de connexions**  
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], ouvrez le package [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] qui contient la destination SAP BW.  
  
2.  Sous l’onglet **Flux de données** , double-cliquez sur la destination SAP BW.  
  
3.  Dans l' **Éditeur de destination SAP BW**, cliquez sur **Gestionnaire de connexions** pour ouvrir la page **Gestionnaire de connexions** de l'éditeur.  
  
## <a name="options"></a>Options  
  
> [!NOTE]  
>  Si vous ne connaissez pas toutes les valeurs requises pour configurer la destination, adressez-vous à votre administrateur SAP.  
  
 **Gestionnaire de connexions SAP BW**  
 Sélectionnez un gestionnaire de connexions existant dans la liste ou créez une nouvelle connexion en cliquant sur **Nouveau**.  
  
 **Nouveau**  
 Créez un gestionnaire de connexions à l’aide de la boîte de dialogue **Gestionnaire de connexions SAP BW** .  
  
 **Test de charge**  
 Réalisez un test du processus de chargement qui utilise les paramètres que vous avez sélectionnés et qui charge zéro ligne.  
  
### <a name="infopackageinfosource-options"></a>Options InfoPackage/InfoSource  
 Vous n'avez pas à connaître ni à entrer ces valeurs à l'avance. Utilisez le bouton **Rechercher** pour rechercher et sélectionner l'InfoPackage approprié. Après avoir sélectionné un InfoPackage, le composant entre les valeurs appropriées pour ces options.  
  
 **InfoPackage**  
 Entrez le nom de l'InfoPackage.  
  
 **InfoSource**  
 Entrez le nom de l'InfoSource.  
  
 **Type**  
 Entrez le caractère unique qui identifie le type de l'InfoSource. Le tableau suivant répertorie les valeurs à un seul caractère acceptables.  
  
|Value|Description|  
|-----------|-----------------|  
|**D**|Données de transaction|  
|**M**|Données maîtres|  
|**T**|Textes|  
|**H**|Données de hiérarchie|  
  
 **Système logique**  
 Entrez le nom du système logique qui est associé à l'InfoPackage.  
  
 **Rechercher**  
 Recherchez l’InfoPackage à l’aide de la boîte de dialogue **Rechercher un InfoPackage** . Pour plus d'informations sur cette boîte de dialogue, consultez [Look Up InfoPackage](../../integration-services/data-flow/look-up-infopackage.md).  
  
### <a name="rfc-destination-options"></a>Options de la destination RFC  
 Vous n'avez pas à connaître ni à entrer ces valeurs à l'avance. Utilisez le bouton **Rechercher** pour rechercher et sélectionner la destination RFC appropriée. Après avoir sélectionné une destination RFC, le composant entre les valeurs appropriées pour ces options.  
  
 **Hôte de passerelle**  
 Entrez le nom du serveur ou l'adresse IP de l'hôte de passerelle. Généralement, le nom ou l'adresse IP est identique au serveur d'applications SAP.  
  
 **Service de passerelle**  
 Entrez le nom du service de passerelle, au format **sapgwNN**, où **NN** représente le numéro du système.  
  
 **ID de programme**  
 Entrez l'ID du programme qui est associé à la destination RFC.  
  
 **Rechercher**  
 Recherchez la destination RFC à l’aide de la boîte de dialogue **Rechercher la destination RFC** . Pour plus d'informations sur cette boîte de dialogue, consultez [Look Up RFC Destination](../../integration-services/data-flow/look-up-rfc-destination.md).  
  
### <a name="create-sap-bw-objects-options"></a>Options de création d'objets SAP BW  
 **Sélectionner type d'objet**  
 Sélectionnez le type d'objet SAP Netweaver BW que vous souhaitez créer. Vous pouvez sélectionner l'un des types suivants :  
  
-   InfoObject  
  
-   InfoCube  
  
-   InfoSource  
  
-   InfoPackage  
  
 **Créer**  
 Créez le type sélectionné d'objet SAP Netweaver BW.  
  
|Type d'objet|Résultats|  
|-----------------|------------|  
|**InfoObject**|Créez un nouvel InfoObject à l'aide de la boîte de dialogue **Créer un nouvel InfoObject** . Pour plus d'informations sur cette boîte de dialogue, consultez [Create New InfoObject](../../integration-services/data-flow/create-new-infoobject.md).|  
|**InfoCube**|Créez un nouvel InfoCube à l'aide de la boîte de dialogue **Créer un InfoCube pour les données de transaction** . Pour plus d'informations sur cette boîte de dialogue, consultez [Create InfoCube for Transaction Data](../../integration-services/data-flow/create-infocube-for-transaction-data.md).|  
|**InfoSource**|Créez un nouvel InfoSource à l'aide de la boîte de dialogue **Créer un InfoSource** , puis avec **Créer un InfoSource pour les données de transaction** ou la boîte de dialogue **Créer un InfoSource pour les données maîtres** . Pour plus d'informations sur ces boîtes de dialogue, consultez [Create InfoSource](../../integration-services/data-flow/create-infosource.md), [Create InfoSource for Transaction Data](../../integration-services/data-flow/create-infosource-for-transaction-data.md) et [Create InfoSource for Master Data](../../integration-services/data-flow/create-infosource-for-master-data.md).|  
|**InfoPackage**|Créez un nouvel InfoPackage à l'aide de la boîte de dialogue **Créer un InfoPackage** . Pour plus d'informations sur cette boîte de dialogue, consultez [Create InfoPackage](../../integration-services/data-flow/create-infopackage.md).|  
  
## <a name="see-also"></a> Voir aussi  
 [Éditeur de destination SAP BW &#40;page Mappages&#41;](../../integration-services/data-flow/sap-bw-destination-editor-mappings-page.md)   
 [Éditeur de destination SAP BW &#40;page Sortie d’erreur&#41;](../../integration-services/data-flow/sap-bw-destination-editor-error-output-page.md)   
 [Éditeur de destination SAP BW &#40;page Avancé&#41;](../../integration-services/data-flow/sap-bw-destination-editor-advanced-page.md)   
 [Aide (F1) sur Microsoft Connector pour SAP BW](../../integration-services/microsoft-connector-for-sap-bw-f1-help.md)  
  
  
