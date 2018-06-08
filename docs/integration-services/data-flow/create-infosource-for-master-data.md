---
title: Créer un InfoSource pour les données maîtres | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: data-flow
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: b52a9a89-8380-4a02-8a83-dcfb46ae860e
caps.latest.revision: 10
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 5e7db39cd46a786961675b394b9d2269a0cb3b47
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-infosource-for-master-data"></a>Créer un InfoSource pour les données maîtres
  Utilisez la boîte de dialogue **Créer un InfoSource pour les données maîtres** pour créer un nouvel InfoSource pour les données maîtres dans le système SAP Netweaver BW.  
  
 Vous pouvez ouvrir la boîte de dialogue **Créer un InfoSource pour les données maîtres** à partir de la page **Gestionnaire de connexions** de **l’Éditeur de destination SAP BW**. Pour en savoir plus sur la destination SAP BW, consultez [Destination SAP BW](../../integration-services/data-flow/sap-bw-destination.md).  
  
> [!IMPORTANT]  
>  La documentation de Microsoft Connector 1.1 pour SAP BW suppose que vous êtes familiarisé avec l'environnement SAP Netweaver BW. Pour plus d'informations sur SAP Netweaver BW, ou sur la configuration des objets et des processus SAP Netweaver BW objets, consultez la documentation SAP.  
  
 **Pour ouvrir la boîte de dialogue Créer un InfoSource pour les données maîtres**  
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], ouvrez le package [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] qui contient la destination SAP BW.  
  
2.  Sous l’onglet **Flux de données** , double-cliquez sur la destination SAP BW.  
  
3.  Dans l' **Éditeur de destination SAP BW**, cliquez sur **Gestionnaire de connexions** pour ouvrir la page **Gestionnaire de connexions** de l'éditeur.  
  
4.  Dans la page **Gestionnaire de connexions** , dans la zone de groupe **Créer des objets SAP BW** , sélectionnez **InfoSource**, puis cliquez sur **Créer**.  
  
5.  Dans la boîte de dialogue **Créer un InfoSource** , sélectionnez **Données maîtres**, puis cliquez sur **OK**.  
  
## <a name="options"></a>Options  
 **Nom de l'InfoObject**  
 Entrez le nom de l'InfoObject sur lequel le nouvel InfoSource doit être basé.  
  
 **Rechercher**  
 Recherchez l'InfoObject. Cette option ouvre la boîte de dialogue **Rechercher un InfoObject** dans laquelle vous pouvez sélectionner l’InfoObject. Pour plus d’informations sur cette boîte de dialogue, consultez [Rechercher un InfoObject](../../integration-services/data-flow/look-up-infoobject.md).  
  
 Après avoir sélectionné un InfoObject, le composant remplit la zone de texte **Nom de l’InfoObject** avec le nom de l’InfoObject sélectionné.  
  
 **Nouveau**  
 Créez un nouvel InfoObject. Cette option ouvre la boîte de dialogue **Créer un nouvel InfoObject** dans laquelle vous pouvez créer l’InfoObject. Pour plus d'informations sur cette boîte de dialogue, consultez [Create New InfoObject](../../integration-services/data-flow/create-new-infoobject.md).  
  
 Après avoir créé un InfoObject, le composant remplit la zone de texte **Nom de l’InfoObject** avec le nom du nouvel InfoObject.  
  
 **Description courte**  
 Entrez une brève description pour le nouvel InfoSource.  
  
 **Description longue**  
 Entrez une longue description pour le nouvel InfoSource.  
  
 **Système source**  
 Sélectionnez le système source à associer au nouvel InfoSource.  
  
 **Application**  
 Entrez le nom de l'application à associer au nouvel InfoSource.  
  
 **Attributs**  
 Indique que les données maîtres sont des attributs.  
  
 **Textes**  
 Indique que les données maîtres sont des attributs.  
  
 **Enregistrer et activer**  
 Enregistrez et activez le nouvel InfoSource.  
  
## <a name="see-also"></a> Voir aussi  
 [Créer un InfoSource](../../integration-services/data-flow/create-infosource.md)   
 [Aide (F1) sur Microsoft Connector pour SAP BW](../../integration-services/microsoft-connector-for-sap-bw-f1-help.md)  
  
  
