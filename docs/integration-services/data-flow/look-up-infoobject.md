---
title: Rechercher un InfoObject | Microsoft Docs
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
ms.assetid: e7f4c132-a5ec-49d8-a964-45775432731f
caps.latest.revision: 10
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 20ae53246cc057d7a454363269a51e839639e355
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="look-up-infoobject"></a>Rechercher un InfoObject
  Utilisez la boîte de dialogue **Rechercher un InfoObject** pour rechercher un InfoObject qui est défini dans le système SAP Netweaver BW. Lorsque la liste d'InfoObjects disponibles apparaît, sélectionnez l'InfoObject de votre choix. La destination SAP BW remplira les options associées à l'aide des valeurs requises.  
  
 La destination SAP BW de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Connector 1.1 pour SAP BW utilise la boîte de dialogue **Rechercher un InfoObject** . Pour en savoir plus sur la destination SAP BW, consultez [Destination SAP BW](../../integration-services/data-flow/sap-bw-destination.md).  
  
> [!IMPORTANT]  
>  La documentation de Microsoft Connector 1.1 pour SAP BW suppose que vous êtes familiarisé avec l'environnement SAP Netweaver BW. Pour plus d'informations sur SAP Netweaver BW, ou sur la configuration des objets et des processus SAP Netweaver BW objets, consultez la documentation SAP.  
  
 **Pour ouvrir la boîte de dialogue Rechercher un InfoObject**  
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], ouvrez le package [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] qui contient la destination SAP BW.  
  
2.  Sous l’onglet **Flux de données** , double-cliquez sur la destination SAP BW.  
  
3.  Dans l' **Éditeur de destination SAP BW**, cliquez sur **Gestionnaire de connexions** pour ouvrir la page **Gestionnaire de connexions** de l'éditeur.  
  
4.  Sur la page **Gestionnaire de connexions** , dans la zone de groupe **Créer des objets SAP BW** , sélectionnez l'une des options suivantes :  
  
    1.  Sélectionnez **InfoCube**. Puis, cliquez sur **Créer**. Dans la boîte de dialogue **Créer un InfoCube pour les données de transaction** , cliquez sur **Rechercher** dans la colonne **IObject** pour l'une des lignes de la liste. Chaque ligne représente une colonne dans le flux de données du package.  
  
    2.  Sélectionnez **InfoSource**. Puis, cliquez sur **Créer**. Dans la boîte de dialogue **Créer un InfoSource** , sélectionnez **Données de transaction**. Dans la boîte de dialogue **Créer un InfoSource pour les données de transaction** , cliquez sur **Rechercher** dans la colonne **IObject** pour l'une des lignes de la liste. Chaque ligne représente une colonne dans le flux de données du package.  
  
    3.  Sélectionnez **InfoSource**. Puis, cliquez sur **Créer**. Dans la boîte de dialogue **Créer un InfoSource** , sélectionnez **Données maîtres**. Dans la boîte de dialogue **Créer un InfoSource pour les données maîtres** , cliquez sur **Rechercher**.  
  
 Vous pouvez également ouvrir la boîte de dialogue **Rechercher un InfoObject** en cliquant sur **Ajouter** dans la section **Attributs** de la boîte de dialogue **Créer un nouvel InfoObject** .  
  
## <a name="lookup-options"></a>Options de recherche  
 Dans les zones de texte du champ de recherche, vous pouvez filtrer les résultats en utilisant le caractère générique astérisque (*), ou en utilisant une chaîne partielle conjointement avec le caractère générique astérisque. Cependant, si vous laissez un champ de recherche vide, le processus de recherche mettra uniquement en correspondance les chaînes vides de ce champ.  
  
 **Caractéristiques**  
 Recherchez des InfoObjects qui représentent des caractéristiques.  
  
 **Unités**  
 Recherchez des InfoObjects qui représentent des unités.  
  
 **Chiffres clés**  
 Recherchez des InfoObjects qui représentent des chiffres clés.  
  
 **Caractéristiques de temps**  
 Recherchez des InfoObjects qui représentent des caractéristiques de temps.  
  
 **Nom**  
 Entrez le nom de l'InfoObject à rechercher, ou un nom partiel avec le caractère générique astérisque (*). Sinon, utilisez le caractère générique astérisque seul pour inclure tous les InfoObjects.  
  
 **Description**  
 Entrez la description, ou une description partielle avec le caractère générique astérisque (*). Sinon, utilisez le caractère générique astérisque seul pour inclure tous les InfoObjects, quelle que soit la description.  
  
 **Rechercher**  
 Recherchez les InfoObjects correspondants qui sont définis dans le système SAP Netweaver BW.  
  
## <a name="lookup-results"></a>Résultats de la recherche  
 Après avoir cliqué sur le bouton Rechercher, une liste des InfoObjects du système SAP Netweaver BW apparaît dans un tableau avec les en-têtes de colonnes suivants.  
  
 **InfoObject**  
 Affiche le nom de l'InfoObject qui est défini dans le système SAP Netweaver BW.  
  
 **Texte (court)**  
 Affiche le texte court associé à l'InfoObject.  
  
 Lorsque la liste d'InfoObjects disponibles apparaît, sélectionnez l'InfoObject de votre choix. La destination remplira les options associées à l'aide des valeurs requises.  
  
## <a name="see-also"></a> Voir aussi  
 [Créer un InfoCube pour les données de transaction](../../integration-services/data-flow/create-infocube-for-transaction-data.md)   
 [Créer un InfoSource](../../integration-services/data-flow/create-infosource.md)   
 [Créer un InfoSource pour les données de transaction](../../integration-services/data-flow/create-infosource-for-transaction-data.md)   
 [Créer un InfoSource pour les données maîtres](../../integration-services/data-flow/create-infosource-for-master-data.md)   
 [Créer un nouvel InfoObject](../../integration-services/data-flow/create-new-infoobject.md)   
 [Éditeur de destination SAP BW &#40;page Gestionnaire de connexions&#41;](../../integration-services/data-flow/sap-bw-destination-editor-connection-manager-page.md)   
 [Aide (F1) sur Microsoft Connector pour SAP BW](../../integration-services/microsoft-connector-for-sap-bw-f1-help.md)  
  
  
