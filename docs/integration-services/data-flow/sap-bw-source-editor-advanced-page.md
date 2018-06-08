---
title: Éditeur de source SAP BW (page Avancé) | Microsoft Docs
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
- sql13.dts.designer.sapbwsource.advanced.f1
ms.assetid: 44f3c991-9e8f-4126-a9a2-2d9da779fb11
caps.latest.revision: 10
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 0e8f5dbf87ec79db858d550636c542173cb66883
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sap-bw-source-editor-advanced-page"></a>Éditeur de source SAP BW (page Avancé)
  Utilisez la page **Avancé** de **l’Éditeur de source SAP BW** pour spécifier la règle de conversion de chaînes et le délai d’attente, et également pour réinitialiser l’état d’un ID de demande particulier.  
  
 Pour en savoir plus sur le composant source SAP BW de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Connector 1.1 pour SAP BW, consultez [Source SAP BW](../../integration-services/data-flow/sap-bw-source.md).  
  
> [!IMPORTANT]  
>  La documentation de Microsoft Connector 1.1 pour SAP BW suppose que vous êtes familiarisé avec l'environnement SAP Netweaver BW. Pour plus d'informations sur SAP Netweaver BW, ou sur la configuration des objets et des processus SAP Netweaver BW objets, consultez la documentation SAP.  
  
> [!IMPORTANT]  
>  Vous devez disposer d'une licence SAP supplémentaire pour extraire des données à partir de SAP Netweaver BW. Vérifiez auprès de SAP.  
  
 **Pour ouvrir la page Gestionnaire de connexions**  
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], ouvrez le package [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] qui contient la source SAP BW.  
  
2.  Sous l’onglet **Flux de données** , double-cliquez sur la source SAP BW.  
  
3.  Dans **l’Éditeur de source SAP BW**, cliquez sur **Avancé** pour ouvrir la page **Avancé** de l’éditeur.  
  
## <a name="options"></a>Options  
  
> [!NOTE]  
>  Si vous ne connaissez pas toutes les valeurs requises pour configurer la source, adressez-vous à votre administrateur SAP.  
  
 **Conversion de chaîne**  
 Spécifiez la règle à appliquer pour la conversion de chaîne.  
  
|Option|Description|  
|------------|-----------------|  
|**Conversion de chaîne automatique**|Convertir toutes les chaînes en **nvarchar** quand le système SAP Netweaver BW est un système Unicode. Sinon, convertir toutes les chaînes en **varchar**.|  
|**Convertir les chaînes en varchar**|Convertir toutes les chaînes en **varchar**.|  
|**Convertir les chaînes en nvarchar**|Convertir les chaînes en **nvarchar**.|  
  
 **Délai d'attente (secondes)**  
 Spécifiez le nombre maximal de secondes pendant lesquelles la source doit attendre.  
  
> [!NOTE]  
>  Cette option n’est valide que si vous avez sélectionné **A - Attendre la notification** comme valeur du **Mode d’exécution** dans la page **Gestionnaire de connexions** de l’éditeur. Pour plus d’informations, consultez [Éditeur de source SAP BW &#40;page Gestionnaire de connexions&#41;](../../integration-services/data-flow/sap-bw-source-editor-connection-manager-page.md).  
  
 **ID de demande**  
 Spécifiez l’ID de demande dont vous voulez réinitialiser l’état sur « V - Vert » quand vous cliquez sur **Réinitialiser**.  
  
 **Réinitialiser**  
 Vous permet de réinitialiser l'état de l'ID de demande spécifié sur « V - Vert », après l'invite de confirmation. Ce peut être utile lorsqu'un problème est survenu et que le système SAP Netweaver BW a marqué la demande avec un état jaune ou rouge.  
  
## <a name="see-also"></a> Voir aussi  
 [Éditeur de source SAP BW &#40;page Gestionnaire de connexions&#41;](../../integration-services/data-flow/sap-bw-source-editor-connection-manager-page.md)   
 [Éditeur de source SAP BW &#40;page Colonnes&#41;](../../integration-services/data-flow/sap-bw-source-editor-columns-page.md)   
 [Éditeur de source SAP BW &#40;page Sortie d’erreur&#41;](../../integration-services/data-flow/sap-bw-source-editor-error-output-page.md)   
 [Aide (F1) sur Microsoft Connector pour SAP BW](../../integration-services/microsoft-connector-for-sap-bw-f1-help.md)  
  
  
