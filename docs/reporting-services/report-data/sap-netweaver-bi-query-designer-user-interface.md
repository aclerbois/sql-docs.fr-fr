---
title: Interface utilisateur du Concepteur de requêtes SAP NetWeaver BI | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-data
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.rtp.rptdesigner.dataview.sapbwquerydesigner.f1
- "10014"
helpviewer_keywords:
- data sources [Reporting Services], SAP NetWeaver Business Intelligence
- SAP NetWeaver Business Intelligence [Reporting Services], query designer
- query designers [Reporting Services]
ms.assetid: 102da66e-ca31-41aa-ab4b-c9b5ab752a72
caps.latest.revision: 38
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 9323e6cf41ff7223634b744900997744c6feb563
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sap-netweaver-bi-query-designer-user-interface"></a>Interface utilisateur du Concepteur de requêtes SAP NetWeaver BI
  [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] fournit un concepteur de requêtes graphique permettant de générer des requêtes MDX (Multidimensional Expression) pour une source de données SAP NetWeaver® Business Intelligence. Le concepteur de requêtes graphique MDX comporte deux modes : le mode Création et le mode Requête. Chaque mode fournit un volet Métadonnées à partir duquel vous pouvez faire glisser des membres à partir d'InfoCube, de MultiProvider ou d'une requête Web définie sur la source de données, afin de générer une requête MDX visant à récupérer des données lors du traitement du rapport.  
  
> [!IMPORTANT]  
>  Les utilisateurs accèdent aux sources de données lorsqu'ils créent et exécutent des requêtes. Vous devez accorder des autorisations minimales sur les sources de données, telles que des autorisations en lecture seule.  
  
 Pour plus d’informations sur l’utilisation d’une source de données multidimensionnelle SAP, consultez [Type de connexion SAP NetWeaver BI &#40;SSRS&#41;](../../reporting-services/report-data/sap-netweaver-bi-connection-type-ssrs.md).  
  
 Cette section décrit les boutons de la barre d'outils et les volets du Concepteur de requêtes pour chaque mode du concepteur de requêtes graphique.  
  
## <a name="graphical-query-designer-in-design-mode"></a>Concepteur de requêtes graphique en mode Création  
 Lorsque vous modifiez une requête de dataset qui utilise une source de données [!INCLUDE[SAP_DPE_BW_1](../../includes/sap-dpe-bw-1-md.md)] , le concepteur de requêtes graphique s'ouvre en mode Création. La figure suivante présente les différents volets du mode Création.  
  
 ![Concepteur de requêtes avec MDX en mode Création](../../reporting-services/report-data/media/rsqd-dssapbw-mdx-designmode.gif "Concepteur de requêtes avec MDX en mode Création")  
  
 Le tableau suivant répertorie les volets disponibles dans ce mode.  
  
|Volet|Fonction|  
|----------|--------------|  
|Bouton Sélection de cube|Affiche l'InfoCube, le MultiProvider ou la requête Web actuellement sélectionné.|  
|Volet Métadonnées|Affiche une liste hiérarchique des InfoCubes, MultiProviders et des requêtes. Les requêtes créées sur la source de données peuvent apparaître sous le cube correspondant.|  
|Volet Membres calculés|Affiche les membres calculés actuellement définis et disponibles pour être utilisés dans la requête.|  
|Volet Données|Affiche les résultats de l'exécution de la requête.|  
  
 Vous pouvez faire glisser des dimensions et des chiffres clés à partir du volet Métadonnées ainsi que des membres calculés à partir du volet Membres calculés dans le volet Données. Si le bouton bascule **Exécution automatique** de la barre d'outils est activé, le Concepteur de requêtes exécute la requête chaque fois que vous déposez un objet dans le volet Données. Si le bouton **Exécution automatique** est désactivé, le Concepteur de requêtes n'exécute pas la requête lorsque vous effectuez des modifications dans le volet Données. Vous pouvez exécuter la requête manuellement en utilisant le bouton **Exécuter** de la barre d'outils.  
  
### <a name="toolbar-for-the-graphical-query-designer-in-design-mode-toolbar"></a>Barre d'outils du concepteur de requêtes graphique en mode Création  
 La barre d'outils du Concepteur de requêtes fournit des boutons qui vous aident à concevoir des requêtes MDX à l'aide de l'interface graphique. Le tableau suivant décrit ces boutons et leurs fonctions.  
  
|Bouton|Description|  
|------------|-----------------|  
|**Modifier en tant que texte**|Bascule entre le Concepteur de requêtes textuel et le concepteur de requêtes graphique. Non disponible pour ce type de source de données.|  
|**Importer**|Importe une requête existante à partir d'un fichier de définition de rapport (.rdl) sur le système de fichiers. Pour plus d’informations, consultez [Datasets incorporés dans le rapport et datasets partagés &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/report-embedded-datasets-and-shared-datasets-report-builder-and-ssrs.md).|  
|![Actualiser les champs du dataset](../../reporting-services/report-data/media/rsqdicon-refreshfields.gif "Actualiser les champs du dataset")|Actualise les métadonnées à partir de la source de données.|  
|![Add calculated member](../../reporting-services/report-data/media/rsqdicon-addcalculatedmember.gif "Add calculated member")|Affiche la boîte de dialogue **Générateur de membres calculés** .|  
|![Bouton bascule pour afficher les cellules vides](../../reporting-services/report-data/media/rsqdicon-showemptycells.gif "Bouton bascule pour afficher les cellules vides")|Affiche ou masque les cellules vides dans le volet Données. (Cela revient à utiliser la clause NON EMPTY dans MDX.)|  
|![Exécuter automatiquement la requête](../../reporting-services/report-data/media/rsqdicon-autoexecute.gif "Exécuter automatiquement la requête")|Exécute automatiquement la requête et affiche le résultat chaque fois qu'une modification est effectuée, par exemple lorsqu'une colonne est supprimée du volet Données. Les résultats s'affichent dans le volet Données.|  
|![Supprimer](../../reporting-services/report-data/media/rsqdicon-delete.gif "Supprimer")|Supprime de la requête la colonne sélectionnée dans le volet Données.|  
|![Icône de la boîte de dialogue Paramètres de la requête](../../reporting-services/report-data/media/iconqueryparameter.gif "Icône de la boîte de dialogue Paramètres de la requête")|Affiche la boîte de dialogue **Variables** . Ce bouton est activé uniquement lorsque le cube sélectionné est un cube de requête (car seuls les cubes de requête prennent en charge les variables). Lorsque vous affectez une valeur par défaut à une variable, un paramètre de rapport correspondant est créé.|  
|![Exécuter la requête](../../reporting-services/report-data/media/rsqdicon-run.gif "Exécuter la requête")|Exécute la requête et affiche les résultats dans le volet Données.|  
|![Annuler la requête](../../reporting-services/report-data/media/rsqdicon-cancel.gif "Annuler la requête")|Annule la requête.|  
|![Basculer en mode Création](../../reporting-services/media/rsqdicon-designmode.gif "Basculer en mode Design")|Bascule entre le mode Création et le mode Requête.|  
  
## <a name="graphical-query-designer-in-query-mode"></a>Concepteur de requêtes graphique en mode Requête  
 Pour basculer vers le mode Requête du concepteur de requêtes graphique, cliquez sur le bouton bascule **Mode Création** dans la barre d'outils.  
  
 L'illustration suivante présente les composants du Concepteur de requêtes en mode Requête.  
  
 ![Concepteur de requêtes SAP BW MDX en mode Requête](../../reporting-services/report-data/media/rsqd-dssapbw-mdx-querymode.gif "Concepteur de requêtes SAP BW MDX en mode Requête")  
  
 Le tableau ci-dessous décrit la fonction de chaque volet.  
  
|Volet|Fonction|  
|----------|--------------|  
|Bouton Sélection de cube|Affiche l'InfoCube, le MultiProvider ou tout autre cube actuellement sélectionnés.|  
|Volet Métadonnées/Fonctions|Affiche une fenêtre à onglets qui présente une liste des métadonnées ou des fonctions disponibles qui peuvent être utilisées pour créer le texte de la requête.|  
|Volet Variables|Affiche les variables actuellement définies et disponibles pour être utilisées dans la requête.|  
|Volet Requête|Affiche le texte de la requête actuelle.|  
|Volet Résultat|Affiche les résultats de la requête.|  
  
 Dans le volet Métadonnées, vous pouvez faire glisser des chiffres clés et des dimensions à partir de l’onglet **Métadonnées** vers le volet Requête MDX. Le nom technique des métadonnées est inséré à l’emplacement du curseur. Vous pouvez faire glisser des fonctions à partir de l'onglet **Fonctions** dans le volet Requête MDX. Lorsque vous exécutez la requête, le volet Résultat affiche les résultats pour la requête MDX actuelle.  
  
 Si votre cube sélectionné est une requête Web, vous serez invité à définir des valeurs statiques par défaut pour les variables existantes. Vous pouvez ensuite faire glisser des variables dans le volet Requête MDX.  
  
 Les volets Métadonnées et Variables affichent des noms conviviaux. Lorsque vous déposez les objets dans le volet Requête MDX, les noms techniques requis par la source de données spécifiée dans la requête MDX s'affichent.  
  
### <a name="toolbar-for-the-graphical-query-designer-in-query-mode"></a>Barre d'outils du concepteur de requêtes graphique en mode Requête  
 La barre d'outils du Concepteur de requêtes fournit des boutons qui vous aident à concevoir des requêtes MDX à l'aide de l'interface graphique. Les boutons de la barre d'outils sont identiques en mode Création et en mode Requête, mais les boutons suivants ne sont pas activés en mode Requête :  
  
-   **Modifier en tant que texte**  
  
-   **Ajouter un membre calculé** (![Add calculated member](../../reporting-services/report-data/media/rsqdicon-addcalculatedmember.gif "Add calculated member"))  
  
-   **Afficher les cellules vides** (![Bouton bascule pour afficher les cellules vides](../../reporting-services/report-data/media/rsqdicon-showemptycells.gif "Bouton bascule pour afficher les cellules vides"))  
  
-   **Exécution automatique** (![Exécuter automatiquement la requête](../../reporting-services/report-data/media/rsqdicon-autoexecute.gif "Exécuter automatiquement la requête"))  
  
-   **Supprimer** (![Supprimer](../../reporting-services/report-data/media/rsqdicon-delete.gif "Supprimer"))  
  
## <a name="see-also"></a> Voir aussi  
 [Créer un dataset partagé ou incorporé &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/create-a-shared-dataset-or-embedded-dataset-report-builder-and-ssrs.md)   
 [Fichier de configuration RSReportDesigner](../../reporting-services/report-server/rsreportdesigner-configuration-file.md)  
  
  
