---
title: Destination SAP BW | Microsoft Docs
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
ms.assetid: a612ed91-b89b-4173-a0b1-0bce381e1e28
caps.latest.revision: 11
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: ae1f55181cdd8f66b218ebcc64d7ad152bedab8c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sap-bw-destination"></a>Destination SAP BW
  La destination SAP BW est le composant de destination de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Connector 1.1 pour SAP BW. Ainsi, la destination SAP BW charge les données du flux de données d'un package [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] dans un système SAP Netweaver BW version 7.  
  
 Cette destination comporte une entrée et une sortie d'erreur.  
  
> [!IMPORTANT]  
>  La documentation de Microsoft Connector 1.1 pour SAP BW suppose que vous êtes familiarisé avec l'environnement SAP Netweaver BW. Pour plus d'informations sur SAP Netweaver BW, ou sur la configuration des objets et des processus SAP Netweaver BW objets, consultez la documentation SAP.  
  
 Pour utiliser la destination SAP BW, vous devez effectuer les tâches suivantes :  
  
-   [Préparer les objets SAP Netweaver BW](#bkmk_Prepare_Objects)  
  
-   [Établir une connexion au système SAP Netweaver BW](#bkmk_Connect_Database)  
  
-   [Configurer la destination SAP BW](#bkmk_Configure_Destination)  
  
##  <a name="bkmk_Prepare_Objects"></a> Préparation des objets SAP Netweaver BW requis par la destination  
 La destination SAP BW nécessite la présence de certains objets dans le système SAP Netweaver BW pour que la destination puisse fonctionner. Si ces objets n'existent pas, vous devez suivre ces étapes pour les créer et les configurer dans le système SAP Netweaver BW.  
  
> [!NOTE]  
>  Pour plus d'informations sur ces objets et ces étapes de configuration, consultez la documentation de SAP Netweaver BW.  
  
1.  Créer un nouveau système source :  
  
    1.  Sélectionnez le type **« Interfaces BAPI intermédiaires/tierces »**.  
  
    2.  Pour **Type de communication avec le système cible**, sélectionnez **Non-Unicode (Paramètres MDMP inactifs)**.  
  
    3.  Attribuez un ID de programme approprié.  
  
2.  Attribuez le système source à un InfoSource.  
  
3.  Créez un InfoPackage.  
  
     L'InfoPackage est l'objet le plus important requis par la destination SAP BW.  
  
 Vous pouvez aussi créer des InfoObjects, des InfoCubes, des InfoSources et des InfoPackages supplémentaires, requis pour prendre en charge le chargement des données dans le système SAP Netweaver BW.  
  
##  <a name="bkmk_Connect_Database"></a> Connexion au système SAP Netweaver BW  
 Pour la connexion au système SAP Netweaver BW version 7, la destination SAP BW utilise le gestionnaire de connexions SAP BW qui fait partie du package [!INCLUDE[msCoName](../../includes/msconame-md.md)] Connector 1.1 pour SAP BW. Le gestionnaire de connexions SAP BW est le seul gestionnaire de connexions [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] que la destination SAP BW peut utiliser.  
  
 Pour plus d'informations sur le gestionnaire de connexions SAP BW, consultez [SAP BW Connection Manager](../../integration-services/connection-manager/sap-bw-connection-manager.md).  
  
##  <a name="bkmk_Configure_Destination"></a> Configuration de la destination SAP BW  
 Vous pouvez configurer la destination SAP BW de plusieurs manières :  
  
-   Recherchez et sélectionnez l'InfoPackage à utiliser pour charger les données.  
  
-   Mappez chaque colonne du flux de données à l'InfoObject approprié dans l'InfoPackage.  
  
-   Spécifiez le nombre de lignes de données qui seront transférées à la fois en configurant la propriété **PackageSize** .  
  
-   Choisissez d'attendre la fin du chargement des données par le système SAP Netweaver BW.  
  
-   Choisissez de ne pas déclencher l'InfoPackage, mais d'attendre une notification indiquant que le système SAP Netweaver BW a démarré le chargement des données.  
  
-   Déclenchez éventuellement une autre chaîne de processus une fois le chargement des données terminé.  
  
-   Créez éventuellement les objets SAP Netweaver BW requis par la destination. Cela inclut la création d'InfoObjects, d'InfoCubes, d'InfoSources et d'InfoPackages.  
  
-   Testez le chargement des données avec les options que vous avez sélectionnées.  
  
 Vous pouvez également activer la journalisation des appels de fonction RFC par la destination. (Cette journalisation diffère de la journalisation facultative que vous pouvez activer sur les packages [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].) Activez la journalisation des appels de fonction RFC lorsque vous configurez le gestionnaire de connexions SAP BW que la destination utilisera. Pour plus d'informations sur la configuration du gestionnaire de connexions SAP BW, consultez [SAP BW Connection Manager](../../integration-services/connection-manager/sap-bw-connection-manager.md).  
  
 Si vous ne connaissez pas toutes les valeurs requises pour configurer la destination, adressez-vous à votre administrateur SAP.  
  
 Pour obtenir la procédure pas à pas qui montre comment configurer et utiliser le gestionnaire de connexions, la source et la destination SAP BW, consultez le livre blanc [Utilisation de SQL Server 2008 Integration Services avec SAP BI 7.0](http://go.microsoft.com/fwlink/?LinkID=137090). Ce livre blanc explique également comment configurer les objets nécessaires dans SAP BW.  
  
### <a name="using-the-ssis-designer-to-configure-the-destination"></a>Utilisation du concepteur SSIS pour configurer la destination  
 Pour plus d'informations sur les propriétés de la destination SAP BW que vous pouvez définir dans le Concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , cliquez sur l'une des rubriques suivantes :  
  
-   [Éditeur de destination SAP BW &#40;page Gestionnaire de connexions&#41;](../../integration-services/data-flow/sap-bw-destination-editor-connection-manager-page.md)  
  
-   [Éditeur de destination SAP BW &#40;page Mappages&#41;](../../integration-services/data-flow/sap-bw-destination-editor-mappings-page.md)  
  
-   [Éditeur de destination SAP BW &#40;page Sortie d’erreur&#41;](../../integration-services/data-flow/sap-bw-destination-editor-error-output-page.md)  
  
-   [Éditeur de destination SAP BW &#40;page Avancé&#41;](../../integration-services/data-flow/sap-bw-destination-editor-advanced-page.md)  
  
 Lorsque vous configurez la destination SAP BW, vous pouvez également utiliser différentes boîtes de dialogue pour rechercher ou pour créer des objets SAP Netweaver BW. Pour plus d'informations sur ces boîtes de dialogue, cliquez sur l'une des rubriques suivantes :  
  
-   [Rechercher un InfoPackage](../../integration-services/data-flow/look-up-infopackage.md)  
  
-   [Créer un nouvel InfoObject](../../integration-services/data-flow/create-new-infoobject.md)  
  
-   [Créer un InfoCube pour les données de transaction](../../integration-services/data-flow/create-infocube-for-transaction-data.md)  
  
-   [Rechercher un InfoObject](../../integration-services/data-flow/look-up-infoobject.md)  
  
-   [Créer un InfoSource](../../integration-services/data-flow/create-infosource.md)  
  
-   [Créer un InfoSource pour les données de transaction](../../integration-services/data-flow/create-infosource-for-transaction-data.md)  
  
-   [Créer un InfoSource pour les données maîtres](../../integration-services/data-flow/create-infosource-for-master-data.md)  
  
-   [Créer un InfoPackage](../../integration-services/data-flow/create-infopackage.md)  
  
## <a name="see-also"></a> Voir aussi  
 [Composants Microsoft Connector 1.1 pour SAP BW](../../integration-services/microsoft-connector-for-sap-bw-components.md)  
  
  
