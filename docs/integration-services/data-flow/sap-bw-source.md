---
title: Source SAP BW | Microsoft Docs
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
ms.assetid: 749afb64-3567-4dc9-8431-783d650c25db
caps.latest.revision: 10
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 6320849a8e1f8104171058d6629b1d6d04675043
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sap-bw-source"></a>Source SAP BW
  La source SAP BW est le composant source de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Connector 1.1 pour SAP BW. Ainsi, la source SAP BW extrait des données d'un système SAP Netweaver BW version 7 et met ces données à la disposition du flux de données dans un package [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
 Cette source comporte une sortie et une sortie d'erreur.  
  
> [!IMPORTANT]  
>  La documentation de Microsoft Connector 1.1 pour SAP BW suppose que vous êtes familiarisé avec l'environnement SAP Netweaver BW. Pour plus d'informations sur SAP Netweaver BW, ou sur la configuration des objets et des processus SAP Netweaver BW objets, consultez la documentation SAP.  
  
> [!IMPORTANT]  
>  Vous devez disposer d'une licence SAP supplémentaire pour extraire des données à partir de SAP Netweaver BW. Vérifiez auprès de SAP.  
  
 Pour utiliser la source SAP BW, vous devez effectuer les tâches suivantes :  
  
-   [Préparer les objets SAP Netweaver BW](#bkmk_Prepare_Objects)  
  
-   [Établir une connexion au système SAP Netweaver BW](#bkmk_Connect_Database)  
  
-   [Configurer la source SAP BW](#bkmk_Configure_Source)  
  
##  <a name="bkmk_Prepare_Objects"></a> Préparation des objets SAP Netweaver BW requis par la source  
 La source SAP BW nécessite la présence de certains objets dans le système SAP Netweaver BW pour que la source puisse fonctionner. Si ces objets n'existent pas, vous devez suivre ces étapes pour les créer et les configurer dans le système SAP Netweaver BW.  
  
> [!NOTE]  
>  Pour plus d'informations sur ces objets et ces étapes de configuration, consultez la documentation de SAP Netweaver BW.  
  
1.  Connectez-vous à SAP Netweaver BW via l'interface GUI SAP, entrez le code de transaction SM59, puis créez une destination RFC :  
  
    1.  Pour **Type de connexion**, sélectionnez **TCP/IP**.  
  
    2.  Pour **Type d'activation**, sélectionnez **Programme de serveur inscrit**.  
  
    3.  Pour **Type de communication avec le système cible**, sélectionnez **Non-Unicode (Paramètres MDMP inactifs)**.  
  
    4.  Attribuez un ID de programme approprié.  
  
2.  Créer une destination Open Hub :  
  
    1.  Accédez à l’atelier de l’administrateur (code de transaction RSA1) puis, dans le volet gauche, sélectionnez **Destination Open Hub**.  
  
    2.  Dans le volet central, cliquez avec le bouton droit sur un InfoArea, puis sélectionnez **Créer une destination Open Hub**.  
  
    3.  Pour **Type de destination**, sélectionnez **Outil tiers**, puis entrez la destination RFC créée précédemment.  
  
    4.  Enregistrez et activez la nouvelle destination Open Hub.  
  
3.  Créer un processus de transfert de données (DTP) :  
  
    1.  Dans le volet central de l’InfoArea, cliquez avec le bouton droit sur la destination créée précédemment, puis sélectionnez **Créer le processus de transfert de données**.  
  
    2.  Configurez, enregistrez et activez le processus DTP.  
  
    3.  Dans le menu, cliquez sur **Atteindre**, puis cliquez sur **Paramètres pour le gestionnaire de lots**.  
  
    4.  Mettez à jour **Nombre de processus** à 1 pour le traitement en série.  
  
4.  Créer une chaîne de processus :  
  
    1.  Lors de la configuration de la chaîne de processus, sélectionnez **Démarrer à l'aide de la chaîne de métadonnées ou de l'API** comme **Options de planification** du **Processus de démarrage**, puis ajoutez le processus DTP créé précédemment comme nœud suivant.  
  
    2.  Enregistrez et activez la chaîne de processus.  
  
     La source SAP BW peut appeler la chaîne de processus pour activer le processus de transfert de données.  
  
##  <a name="bkmk_Connect_Database"></a> Connexion au système SAP Netweaver BW  
 Pour la connexion au système SAP Netweaver BW version 7, la source SAP BW utilise le gestionnaire de connexions SAP BW qui fait partie du package [!INCLUDE[msCoName](../../includes/msconame-md.md)] Connector 1.1 pour SAP BW. Le gestionnaire de connexions SAP BW est le seul gestionnaire de connexions [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] que la source SAP BW peut utiliser.  
  
 Pour plus d'informations sur le gestionnaire de connexions SAP BW, consultez [SAP BW Connection Manager](../../integration-services/connection-manager/sap-bw-connection-manager.md).  
  
##  <a name="bkmk_Configure_Source"></a> Configuration de la source SAP BW  
 Vous pouvez configurer la source SAP BW comme suit :  
  
-   Recherchez et sélectionnez la destination OHS (Open Hub Service) à utiliser pour extraire des données.  
  
-   Sélectionnez l'une des méthodes suivantes pour extraire des données :  
  
    -   Déclenchez une chaîne de processus. Dans ce cas, le package [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] démarre le processus d'extraction.  
  
    -   Attendez la notification du système SAP Netweaver BW pour démarrer une extraction. Dans ce cas, le système SAP Netweaver BW démarre le processus d'extraction.  
  
    -   Récupérez les données associées à un ID de demande particulier. Dans ce cas, le système SAP Netweaver BW a déjà extrait les données dans une table interne et le package à [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] se contente de lire les données.  
  
-   Selon la méthode sélectionnée pour extraire des données, fournissez les informations supplémentaires suivantes :  
  
    -   Pour l’option **P - Déclencher une chaîne de processus** , fournissez le nom d’hôte de passerelle, le nom du service de passerelle, l’ID de programme pour la destination RFC et le nom de la chaîne de processus.  
  
    -   Pour l’option **A - Attendre la notification** , fournissez le nom d’hôte de passerelle, le nom du serveur de passerelle et l’ID de programme pour la destination RFC. Vous pouvez également spécifier le délai d'attente (en secondes). Le délai d'attente représente la durée maximale pendant laquelle la source attendra une notification.  
  
    -   Pour l’option **E - Extraire uniquement** , fournissez l’ID de demande.  
  
-   Spécifiez les règles pour la conversion de chaînes. (Par exemple, convertissez toutes les chaînes, selon que le système SAP Netweaver BW est Unicode ou non, ou convertissez toutes les chaînes en **varchar** ou **nvarchar**).  
  
-   Utilisez les options que vous avez sélectionnées pour afficher un aperçu des données à extraire.  
  
 Vous pouvez également activer la journalisation des appels de fonction RFC par la source. (Cette journalisation diffère de la journalisation facultative que vous pouvez activer sur les packages [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].) Vous activez la journalisation des appels de fonction RFC lorsque vous configurez le gestionnaire de connexions SAP BW que la source utilisera. Pour plus d'informations sur la configuration du gestionnaire de connexions SAP BW, consultez [SAP BW Connection Manager](../../integration-services/connection-manager/sap-bw-connection-manager.md).  
  
 Si vous ne connaissez pas toutes les valeurs requises pour configurer la source, adressez-vous à votre administrateur SAP.  
  
 Pour obtenir la procédure pas à pas qui montre comment configurer et utiliser le gestionnaire de connexions, la source et la destination SAP BW, consultez le livre blanc [Utilisation de SQL Server 2008 Integration Services avec SAP BI 7.0](http://go.microsoft.com/fwlink/?LinkID=137090). Ce livre blanc explique également comment configurer les objets nécessaires dans SAP BW.  
  
### <a name="using-the-ssis-designer-to-configure-the-source"></a>Utilisation du concepteur SSIS pour configurer la source  
 Pour plus d'informations sur les propriétés de la source SAP BW que vous pouvez définir dans le Concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , cliquez sur l'une des rubriques suivantes :  
  
-   [Éditeur de source SAP BW &#40;page Gestionnaire de connexions&#41;](../../integration-services/data-flow/sap-bw-source-editor-connection-manager-page.md)  
  
-   [Éditeur de source SAP BW &#40;page Colonnes&#41;](../../integration-services/data-flow/sap-bw-source-editor-columns-page.md)  
  
-   [Éditeur de source SAP BW &#40;page Sortie d’erreur&#41;](../../integration-services/data-flow/sap-bw-source-editor-error-output-page.md)  
  
-   [Éditeur de source SAP BW &#40;page Avancé&#41;](../../integration-services/data-flow/sap-bw-source-editor-advanced-page.md)  
  
 Pendant que vous configurez la source SAP BW, vous pouvez également utiliser différentes boîtes de dialogue pour rechercher des objets SAP Netweaver BW ou pour afficher un aperçu des données sources. Pour plus d'informations sur ces boîtes de dialogue, cliquez sur l'une des rubriques suivantes :  
  
-   [Rechercher la destination RFC](../../integration-services/data-flow/look-up-rfc-destination.md)  
  
-   [Rechercher la chaîne de processus](../../integration-services/data-flow/look-up-process-chain.md)  
  
-   [Journal des requêtes](../../integration-services/data-flow/request-log.md)  
  
-   [Aperçu](../../integration-services/data-flow/preview.md)  
  
## <a name="see-also"></a> Voir aussi  
 [Composants Microsoft Connector 1.1 pour SAP BW](../../integration-services/microsoft-connector-for-sap-bw-components.md)  
  
  
