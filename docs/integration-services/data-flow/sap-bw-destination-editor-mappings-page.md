---
title: Éditeur de destination SAP BW (page Mappages) | Microsoft Docs
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
- sql13.dts.designer.sapbwdestination.columns.f1
ms.assetid: dfa1f1d6-6b64-4331-bdc5-eaa8b7aa41a1
caps.latest.revision: 11
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: f0d0c2f6c12982eb6c2c9d23de4844ce602b2386
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sap-bw-destination-editor-mappings-page"></a>Éditeur de destination SAP BW (page Mappages)
  La page **Mappages** de **l’Éditeur de destination SAP BW** vous permet de mapper les colonnes d’entrée aux colonnes de destination.  
  
 Pour en savoir plus sur la destination SAP BW de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Connector 1.1 pour SAP BW, consultez [Destination SAP BW](../../integration-services/data-flow/sap-bw-destination.md).  
  
> [!IMPORTANT]  
>  La documentation de Microsoft Connector 1.1 pour SAP BW suppose que vous êtes familiarisé avec l'environnement SAP Netweaver BW. Pour plus d'informations sur SAP Netweaver BW, ou sur la configuration des objets et des processus SAP Netweaver BW objets, consultez la documentation SAP.  
  
 **Pour ouvrir la page Mappages**  
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], ouvrez le package [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] qui contient la destination SAP BW.  
  
2.  Sous l’onglet **Flux de données** , double-cliquez sur la destination SAP BW.  
  
3.  Dans **l’Éditeur de destination SAP BW**, cliquez sur **Mappages** pour ouvrir la page **Mappages** de l’éditeur.  
  
## <a name="options"></a>Options  
  
> [!NOTE]  
>  Si vous ne connaissez pas toutes les valeurs requises pour configurer la destination, adressez-vous à votre administrateur SAP.  
  
 La page **Mappages** de **l’Éditeur de destination SAP BW** contient deux sections :  
  
-   La section supérieure montre les colonnes d'entrée et de destination disponibles et vous permet de créer des mappages entre ces deux types de colonnes.  
  
-   La section inférieure est un tableau qui indique quelles colonnes d'entrée ont été mappées à quelles colonnes de sortie.  
  
### <a name="upper-section-options"></a>Options de la section supérieure  
 La section supérieure comporte les options suivantes :  
  
 **Colonnes d'entrée disponibles**  
 Affichez la liste des colonnes d'entrée disponibles.  
  
 Pour mapper une colonne d’entrée à une colonne de destination, faites glisser une colonne de la liste **Colonnes d’entrée disponibles** et déposez-la sur une colonne de la liste **Colonnes de destination disponibles** .  
  
 **Colonnes de destination disponibles**  
 Affichez la liste des colonnes de destination disponibles.  
  
 Pour mapper une colonne d’entrée à une colonne de destination, faites glisser une colonne de la liste **Colonnes d’entrée disponibles** et déposez-la sur une colonne de la liste **Colonnes de destination disponibles** .  
  
 La section supérieure comporte également un menu contextuel que vous pouvez ouvrir en cliquant avec le bouton droit sur l'arrière-plan. Ce menu contextuel contient les options suivantes :  
  
-   **Sélectionner tous les mappages**  
  
-   **Supprimer les mappages sélectionnés**  
  
-   **Mapper les éléments par noms correspondants**  
  
### <a name="lower-section-columns"></a>Colonnes de la section inférieure  
 La section inférieure est un tableau des mappages. Elle comporte les colonnes suivantes :  
  
 **Colonne d'entrée**  
 Affichez les colonnes d'entrée que vous avez sélectionnées.  
  
 Pour mapper une colonne d'entrée différente à la même colonne de destination, sélectionnez une colonne d'entrée différente dans la liste. Pour supprimer un mappage, sélectionnez **\<ignorer>** pour exclure la colonne d’entrée de la sortie.  
  
 **Colonne de destination**  
 Affichez chaque colonne de destination disponible, qu'elle soit mappée ou non.  
  
## <a name="see-also"></a> Voir aussi  
 [Éditeur de destination SAP BW &#40;page Gestionnaire de connexions&#41;](../../integration-services/data-flow/sap-bw-destination-editor-connection-manager-page.md)   
 [Éditeur de destination SAP BW &#40;page Sortie d’erreur&#41;](../../integration-services/data-flow/sap-bw-destination-editor-error-output-page.md)   
 [Éditeur de destination SAP BW &#40;page Avancé&#41;](../../integration-services/data-flow/sap-bw-destination-editor-advanced-page.md)   
 [Aide (F1) sur Microsoft Connector pour SAP BW](../../integration-services/microsoft-connector-for-sap-bw-f1-help.md)  
  
  
