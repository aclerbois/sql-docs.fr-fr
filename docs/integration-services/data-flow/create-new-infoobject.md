---
title: Créer un nouvel InfoObject | Microsoft Docs
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
ms.assetid: 3587a633-1c0b-4d63-a22a-6b2b93923c3a
caps.latest.revision: 10
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 185c7acee5371b1cd2eb0bfa6df3b2ed3f8a251a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-new-infoobject"></a>Créer un nouvel InfoObject
  Utilisez la boîte de dialogue **Créer un nouvel InfoObject** pour créer un InfoObject dans le système SAP Netweaver BW.  
  
 Vous pouvez ouvrir la boîte de dialogue **Créer un nouvel InfoObject** à partir de la page **Gestionnaire de connexions** de **l’Éditeur de destination SAP BW**. Pour en savoir plus sur la destination SAP BW, consultez [Destination SAP BW](../../integration-services/data-flow/sap-bw-destination.md).  
  
> [!IMPORTANT]  
>  La documentation de Microsoft Connector 1.1 pour SAP BW suppose que vous êtes familiarisé avec l'environnement SAP Netweaver BW. Pour plus d'informations sur SAP Netweaver BW, ou sur la configuration des objets et des processus SAP Netweaver BW objets, consultez la documentation SAP.  
  
 **Pour ouvrir la boîte de dialogue Créer un nouvel InfoObject**  
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], ouvrez le package [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] qui contient la destination SAP BW.  
  
2.  Sous l’onglet **Flux de données** , double-cliquez sur la destination SAP BW.  
  
3.  Dans l' **Éditeur de destination SAP BW**, cliquez sur **Gestionnaire de connexions** pour ouvrir la page **Gestionnaire de connexions** de l'éditeur.  
  
4.  Dans la page **Gestionnaire de connexions** , dans la zone de groupe **Créer des objets SAP BW** , procédez de l’une des façons suivantes pour créer un InfoObject :  
  
    1.  Pour créer un InfoObject directement, sélectionnez **InfoObject**, puis cliquez sur **Créer** pour ouvrir la boîte de dialogue **Créer un nouvel InfoObject** .  
  
    2.  Pour créer un InfoObject lors de la création d’un InfoCube, sélectionnez **InfoCube**, puis cliquez sur **Créer**. Dans la boîte de dialogue **Créer un InfoCube pour les données de transaction** , dans la colonne **IObject** pour l’une des lignes de la liste, sélectionnez **Créer** pour ouvrir la boîte de dialogue **Créer un nouvel InfoObject** .  
  
        > [!NOTE]  
        >  Chaque ligne de la table représente une colonne dans le flux de données du package.  
  
    3.  Pour créer un InfoObject lors de la création d’un InfoSouce pour les données transactionnelles, sélectionnez **InfoSource**, puis cliquez sur **Créer**. Dans la boîte de dialogue **Créer un InfoSource** , sélectionnez **Données de transaction**, puis cliquez sur **OK**. Dans la boîte de dialogue **Créer un InfoSource pour les données de transaction** , dans la colonne **IObject** pour l’une des lignes de la liste, sélectionnez **Créer** pour ouvrir la boîte de dialogue **Créer un nouvel InfoObject** .  
  
        > [!NOTE]  
        >  Chaque ligne de la table représente une colonne dans le flux de données du package.  
  
    4.  Pour créer un InfoObject lors de la création d’un InfoSource pour les données maîtres, sélectionnez **InfoSource**, puis cliquez sur **Créer**. Dans la boîte de dialogue **Créer un InfoSource** , sélectionnez **Données maîtres**, puis cliquez sur **OK**. Dans la boîte de dialogue **Créer un InfoSource pour les données maîtres** , cliquez sur **Nouveau** pour ouvrir la boîte de dialogue **Créer un nouvel InfoObject** .  
  
 Vous pouvez également ouvrir la boîte de dialogue **Créer un nouvel InfoObject** en cliquant sur **Nouveau** dans la section **Attributs** de la boîte de dialogue **Créer un nouvel InfoObject** .  
  
## <a name="general-options"></a>Options générales  
 **Caractéristique**  
 Créez un InfoObject qui représente des données de dimension.  
  
 **Chiffre clé**  
 Créez un InfoObject qui représente des données de faits.  
  
 **Nom de l'InfoObject**  
 Entrez le nom de l'InfoObject.  
  
 **Description courte**  
 Entrez une brève description pour l'InfoObject.  
  
 **Description longue**  
 Entrez une longue description pour l'InfoObject.  
  
 **Comporte des données maîtres**  
 Indique que l'InfoObject contient des données maîtres sous la forme d'attributs, de textes ou de hiérarchies.  
  
> [!NOTE]  
>  Sélectionnez cette option si l’InfoObject représente des données dimensionnelles et que vous avez sélectionné l’option **Caractéristique** .  
  
 **Autoriser les minuscules**  
 Autorisez les minuscules dans les données de l'InfoObject.  
  
 **Enregistrer et activer**  
 Enregistrez et activez le nouvel InfoObject.  
  
## <a name="data-type-options"></a>Options de type de données  
 **CHAR - Chaîne de caractères**  
 Indique que l'InfoObject contient des données caractères.  
  
 **NUMC - Chaîne numérique**  
 Indique que l'InfoObject contient des données numériques.  
  
 **DATS - Date (AAAMMJJ)**  
 Indique que l'InfoObject contient des données de date.  
  
 **TIMS - Heure (HHMMSS)**  
 Indique que l'InfoObject contient des données d'heure.  
  
 **Longueur**  
 Entrez la longueur des données.  
  
## <a name="text-options"></a>Options de texte  
 **Avec textes**  
 Indique que l'InfoObject contient des textes.  
  
 **Texte court**  
 Indique que l'InfoObject contient des textes courts.  
  
 **Texte moyen**  
 Indique que l'InfoObject contient des textes moyens.  
  
 **Texte long**  
 Indique que l'InfoObject contient des textes longs.  
  
 **Texte dépendant de la langue**  
 Indique que les textes sont dépendants de la langue.  
  
 **Texte dépendant de l'heure**  
 Indique que les textes sont dépendants de l'heure.  
  
## <a name="attributes-section"></a>Section Attributs  
 La section **Attributs** se compose d’une liste d’attributs pour l’InfoObject et des options qui vous permettent d’ajouter et de supprimer des attributs dans la liste.  
  
### <a name="attributes-list"></a>Liste des attributs  
 La liste **Attributs** affiche les attributs de l’InfoObject en cours de création. La liste **Attributs** comporte les en-têtes de colonnes suivants :  
  
 **InfoObject**  
 Affichez le nom de l'InfoObject.  
  
 **Description**  
 Affichez la description de l'InfoObject.  
  
 **Type d'InfoObject**  
 Affichez le type de l'InfoObject. Le tableau suivant répertorie les valeurs possibles pour le type.  
  
|Valeur|Description|  
|-----------|-----------------|  
|CHA|Caractéristiques|  
|KYF|Chiffres clés|  
|UNI|Unités|  
|TIM|Caractéristiques de temps|  
  
### <a name="attributes-options"></a>Options des attributs  
 Utilisez les options suivantes pour ajouter et supprimer des attributs pour l'InfoObject en cours de création :  
  
 **Ajouter**  
 Ajoutez un InfoObject existant comme attribut.  
  
 Pour ajouter un InfoObject existant, cliquez sur Ajouter, puis utilisez la boîte de dialogue **Rechercher un InfoObject** pour trouver l’InfoObject. Pour plus d’informations sur cette boîte de dialogue, consultez [Rechercher un InfoObject](../../integration-services/data-flow/look-up-infoobject.md).  
  
 **Nouveau**  
 Ajoutez un nouvel InfoObject comme attribut.  
  
 Pour créer et ajouter un nouvel InfoObject, cliquez sur Nouveau, puis utilisez une nouvelle instance de la boîte de dialogue **Créer un nouvel InfoObject** pour créer l’InfoObject.  
  
 **Supprimer**  
 Supprimez l’InfoObject sélectionné de la liste **Attributs** .  
  
## <a name="see-also"></a> Voir aussi  
 [Créer un InfoCube pour les données de transaction](../../integration-services/data-flow/create-infocube-for-transaction-data.md)   
 [Créer un InfoSource](../../integration-services/data-flow/create-infosource.md)   
 [Créer un InfoSource pour les données de transaction](../../integration-services/data-flow/create-infosource-for-transaction-data.md)   
 [Créer un InfoSource pour les données maîtres](../../integration-services/data-flow/create-infosource-for-master-data.md)   
 [Aide (F1) sur Microsoft Connector pour SAP BW](../../integration-services/microsoft-connector-for-sap-bw-f1-help.md)  
  
  
