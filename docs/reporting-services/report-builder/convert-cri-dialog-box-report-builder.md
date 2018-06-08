---
title: Boîte de dialogue de conversion des éléments de rapport personnalisés (Générateur de rapports) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-builder
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
f1_keywords:
- "10008"
helpviewer_keywords:
- CRI
- custom report items
ms.assetid: 2a3f2ac6-667e-4498-8b73-9c40beb993f5
caps.latest.revision: 18
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: acf14f5921a63b5fb27eb9027afb10235d590172
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="convert-cri-dialog-box-report-builder"></a>Boîte de dialogue de conversion des éléments de rapport personnalisés (Générateur de rapports)
  Ce rapport contient des éléments de rapport personnalisés (CRI, Custom Report Item) avec des fonctionnalités non prises en charge. Les éléments de rapport personnalisés sont des extensions du langage RDL (Report Definition Language) qui prennent en charge les objets personnalisés qui affichent les données dans un rapport. Les CRI incluent des composants du moment du design et du moment de l'exécution fournis par des éditeurs de logiciels tiers.  
  
> [!NOTE]  
>  Le choix de prendre en charge des éléments de rapport personnalisés sur un serveur de rapports est une décision qui revient à l'administrateur système. Pour permettre l'affichage des éléments de rapport personnalisés (CRI) dans un rapport, les composants CRI doivent être installés sur le client de création de rapports pour l'aperçu d'un rapport et sur le serveur de rapports pour l'affichage d'un rapport publié et téléchargé.  
  
 Certains CRI peuvent être convertis en éléments de rapport dans le nouveau format de définition de rapport. Lorsque vous ouvrez le rapport, vous êtes invité à effectuer une mise à niveau. À partir des informations suivantes, déterminez s'il est nécessaire de convertir les CRI dans ce rapport :  
  
-   **Oui** Choisissez **Oui** pour convertir tous les CRI du rapport, dans la mesure du possible. Les fonctionnalités non prises en charge dans les CRI ne peuvent pas être mises à niveau et peuvent être supprimées du fichier de définition du rapport. Pour obtenir la liste des fonctionnalités non prises en charge, consultez [Mettre à niveau des rapports](../../reporting-services/install-windows/upgrade-reports.md). Lorsque vous consultez le rapport, vous pouvez voir des différences dans la manière dont les CRI apparaissent dans le rapport.  
  
-   **Non** Choisissez **Non** si vous ne souhaitez pas convertir les CRI dans le rapport. Ces CRI ne peuvent pas être affichés par le processeur de rapports dans leur version actuelle. Si votre administrateur système projette d’installer une nouvelle version du CRI de l’éditeur de logiciels tiers compatible avec le nouveau format de définition de rapport, vous devez choisir **Non**. Tant que de nouvelles versions ne sont pas disponibles, les CRI apparaissent dans le rapport sous la forme d'une zone de texte vide marquée d'une croix (X) rouge.  
  
 Dans les deux cas, le rapport est mis à niveau vers le nouveau format de définition de rapport et une copie de sauvegarde du rapport d’origine est enregistrée au format *\<nom rapport>* `-` Backup.rdl. Si vous enregistrez le rapport dans votre outil de création de rapports, vous enregistrez le rapport mis à niveau dans le nouveau format de définition de rapport. Si vous publiez le rapport, celui-ci est d'abord enregistré sur votre ordinateur, puis publié sur le serveur de rapports. Vous publiez la version mise à niveau du rapport sur le serveur de rapports.  
  
 Si vous n'enregistrez pas le rapport, le rapport d'origine reste inchangé. Toutefois, vous ne pouvez pas modifier ce rapport dans une version ultérieure de [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] , ni dans un environnement de création de rapports qui utilise ce format de définition de rapport. Vous pouvez continuer à exécuter la version d'origine du rapport en téléchargeant ce dernier sur un serveur de rapports [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] ou version ultérieure, à l'aide du Gestionnaire de rapports. Pour plus d’informations, consultez [Charger un fichier ou un rapport &#40;Gestionnaire de rapports&#41;](../../reporting-services/reports/upload-a-file-or-report-report-manager.md).  
  
 Pour les rapports que vous téléchargez au lieu de les publier sur un serveur de rapports, le processeur de rapports détermine si le rapport peut être mis à niveau à la première utilisation. Les rapports qui ne peuvent pas être mis à niveau sont traités en mode de compatibilité descendante et continuent à s'afficher comme dans la version antérieure de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. Pour plus d'informations, consultez [Mettre à niveau des rapports](../../reporting-services/install-windows/upgrade-reports.md).  
  
 Pour identifier le format de définition de rapport actuel d’un rapport, d’un serveur de rapports ou de votre environnement de création de rapports, consultez [Rechercher la version du schéma de définition de rapport &#40;SSRS&#41;](../../reporting-services/reports/find-the-report-definition-schema-version-ssrs.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Aide du Générateur de rapports pour les boîtes de dialogue, les volets et les Assistants](http://msdn.microsoft.com/en-us/2da24891-0b6d-4d3c-8b18-81b98752642f)  
  
  
