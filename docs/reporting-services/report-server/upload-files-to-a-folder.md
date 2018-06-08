---
title: Télécharger des fichiers dans un dossier | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-server
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- publishing reports [Reporting Services], uploading files
- reports [Reporting Services], publishing
- uploading reports [Reporting Services]
- uploading files [Reporting Services]
- files [Reporting Services], uploading
- files [Reporting Services]
- folders [Reporting Services], uploading files to
ms.assetid: 2f99a288-d4aa-4c64-b310-e457a2aef2c5
caps.latest.revision: 42
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: dc7ec7802dc229fe3272f6204d3aede698ede150
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="upload-files-to-a-folder"></a>Télécharger des fichiers dans un dossier
  Vous pouvez télécharger des fichiers à partir du système de fichiers, puis les stocker comme éléments gérés dans une base de données de serveur de rapports. Ce qui se produit lorsque vous téléchargez un fichier dépend du type du fichier.  
  
-   Le téléchargement d'un fichier .rdl équivaut à la publication d'un rapport.  
  
-   Le téléchargement de n'importe quel autre fichier l'ajoute à la base de données du serveur de rapports en tant qu'objet binaire unique. Ces fichiers sont publiés sur un serveur de rapports en tant que ressource. Les ressources peuvent être n'importe quel type de fichier. Si l'extension de fichier correspond à un type MIME connu, une icône associée à ce type MIME est utilisée pour identifier le type de la ressource. Sinon, une icône de fichier générique indique la présence d'une ressource.  
  
> [!NOTE]  
>  Vous pouvez télécharger un fichier de source de données de rapports (.rds) pour créer une source de données partagée. Les fichiers .rds sont utilisables uniquement dans le Générateur de rapports. Ils ne peuvent en aucun cas fournir le contenu d'un élément de source de données partagée que vous définissez et gérez via le Gestionnaire de rapports. Outre le téléchargement, vous pouvez écrire un script qui crée une source de données partagée en se basant sur un fichier .rds.  
  
 La taille de fichier maximale des éléments téléchargés est déterminée par [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)]. Par défaut, la taille maximale est de 4 mégaoctets (Mo).  
  
 Sur un plan visuel, les fichiers que vous téléchargez dans une base de données de serveur de rapports s'affichent dans l'arborescence des dossiers avec les icônes suivantes.  
  
 ![Icône Rapport](../../reporting-services/report-server/media/hlp-16doc.gif "Icône Rapport")  
icône de rapport  
  
 ![Icône de modèle](../../reporting-services/report-server/media/model-icon.gif "Icône de modèle")  
icône de modèle de rapport  
  
 ![Icône Ressource générique](../../reporting-services/report-server/media/hlp-16file.gif "Icône Ressource générique")  
icône de ressource générique  
  
 Lorsque vous téléchargez un fichier, il est toujours placé dans le dossier en cours de sélection. Vous pouvez soit naviguer d'abord vers le dossier où l'élément sera stocké, soit télécharger un fichier puis le déplacer ultérieurement vers son emplacement définitif.  
  
 Pour télécharger un fichier, utilisez le Gestionnaire de rapports. Votre possibilité de télécharger des fichiers sur un serveur de rapports dépend des tâches incluses dans votre attribution de rôle. Si vous utilisez la sécurité par défaut, les administrateurs locaux peuvent ajouter des éléments à un serveur de rapports. Si la fonctionnalité Mes rapports est activée, tout utilisateur disposant d'un dossier Mes rapports est autorisé à télécharger des éléments dans ce dossier. Si vous utilisez des attributions de rôle personnalisées, l'attribution de rôle doit inclure les tâches prenant en charge la gestion de dossiers.  
  
|Pour effectuer cette opération|Incluez ces tâches|  
|----------------|-------------------------|  
|Télécharger un fichier .rdl dans un dossier|Gérer les rapports|  
|Télécharger un fichier en tant qu'objet binaire|Gérer les ressources|  
|Afficher le contenu d'un dossier|Afficher les ressources, afficher les rapports|  
  
## <a name="see-also"></a> Voir aussi  
 [Gestionnaire de rapports &#40;SSRS en mode natif&#41;](http://msdn.microsoft.com/library/80949f9d-58f5-48e3-9342-9e9bf4e57896)   
 [Octroi d'autorisations sur un serveur de rapports en mode natif](../../reporting-services/security/granting-permissions-on-a-native-mode-report-server.md)   
 [Tâches et autorisations](../../reporting-services/security/tasks-and-permissions.md)   
 [Télécharger un fichier ou un rapport &#40;Gestionnaire de rapports&#41;](../../reporting-services/reports/upload-a-file-or-report-report-manager.md)  
  
  
