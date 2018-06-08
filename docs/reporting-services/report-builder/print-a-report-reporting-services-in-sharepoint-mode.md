---
title: Imprimer un rapport (Reporting Services en mode SharePoint) | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-builder
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- printing reports, SharePoint Web application
- printing reports
ms.assetid: 026784f7-8cb4-4351-93ee-230b2ab0f8f5
caps.latest.revision: 11
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: 07e50f3dcc37738206fe396b8a68105fd4ac1c87
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="print-a-report-reporting-services-in-sharepoint-mode"></a>Imprimer un rapport (Reporting Services en mode SharePoint)
  Pour un serveur de rapports qui s'exécute en mode SharePoint, il existe trois manières d'imprimer un rapport à partir d'une application Web SharePoint :  
  
-   **À partir d’un site SharePoint** Sélectionnez **Imprimer** dans le menu **Actions** qui s’affiche dans la barre d’outils de rapport lorsque vous ouvrez le rapport. Cela confère des fonctionnalités d’impression à Reporting Services, qui incluent une boîte de dialogue **Imprimer** standard dans laquelle vous pouvez sélectionner une imprimante, spécifier les pages et les marges, et afficher l’aperçu du rapport. La fonctionnalité d'impression est étendue pour être utilisée à la place de la commande Imprimer du menu Fichier d'un navigateur. Lorsque vous choisissez cette méthode d'impression, le rapport est imprimé tel qu'il a été créé, sans les éléments supplémentaires visibles lors de l'impression d'une page Web.  
  
-   **À partir d’un navigateur** Les fonctionnalités d’impression d’un navigateur sont les plus appropriées pour les rapports HTML contenus sur une page simple. En règle générale, les pages que vous imprimez à partir d'un navigateur comprennent tous les éléments visuels présents sur une page Web, ainsi que des informations d'en-tête et de pied de page qui identifient la page ou le site Web. Lorsque vous imprimez à partir d'un navigateur, seul le contenu de la fenêtre active est imprimé. Si le rapport est long, le navigateur n'en imprime qu'une partie (en règle générale, la première page seulement est imprimée).  
  
-   **À partir d’une application cible** Vous pouvez exporter un rapport pour utiliser les fonctionnalités d’impression d’une application cible, telle que Microsoft Office Excel ou Adobe Acrobat Reader. Certains formats d'application, notamment TIFF ou PDF, sont parfaits pour imprimer des rapports de plusieurs pages. Lorsque vous exportez un rapport vers une application bureautique, vous pouvez utiliser toutes les fonctionnalités d'impression spécialisées que l'application prend en charge. Pour exporter un rapport, cliquez sur **Exporter** dans le menu **Actions** qui s’affiche dans la barre d’outils de rapport lorsque vous ouvrez ce dernier.  
  
> [!NOTE]  
>  Pour imprimer un rapport, vous devez être autorisé à l'afficher.  
  
 Pour obtenir des résultats optimaux lors de l’impression d’un rapport à partir d’une page web, utilisez la commande **Imprimer** du menu **Actions** . L’action **Imprimer** est liée à un contrôle d’impression client qui est téléchargé à partir du serveur de rapports. Le téléchargement n’a lieu qu’une seule fois, lorsque vous cliquez sur **Imprimer**pour la première fois.  
  
 Les utilisateurs qui créent des rapports peuvent les concevoir spécialement pour une impression ou pour un format d'application spécifique. Notez qu'en raison de la façon dont la pagination est implémentée pour les différents formats d'application, vous risquez de ne pas obtenir un résultat optimal identique à l'impression selon les rapports et les formats d'exportation. Contrairement aux rapports destinés à l'impression, les pages de rapport affichées à l'écran sont conçues pour accepter des quantités variables de données. Par exemple, les rapports qui comprennent une matrice peuvent entraîner l'agrandissement d'une page à la fois horizontalement et verticalement, selon la façon dont vous développez les lignes et les colonnes. Lors de l'impression d'un rapport de taille variable, un utilisateur qui ne développe pas une matrice obtiendra des résultats différents lors de l'impression. Pour la plupart des rapports exportés, les rapports imprimés incluent tout ce qui est visible sur les rapports, tel que l'utilisateur peut les voir sur un moniteur d'ordinateur.  
  
### <a name="how-to-print-reports-from-the-actions-menu"></a>Comment imprimer des rapports à partir du menu Actions  
  
1.  Ouvrez le rapport.  
  
2.  Dans le menu **Actions** , cliquez sur **Imprimer**. Si vous ne voyez pas le menu **Actions** , cela signifie que la barre d’outils de rapport est masquée et que vous ne pouvez pas utiliser ses fonctionnalités. Si le menu **Actions** est visible mais n’inclut pas la commande **Imprimer** , cela signifie que la fonctionnalité d’impression a été désactivée sur le serveur de rapports et que vous ne pouvez pas l’utiliser.  
  
3.  Dans la boîte de dialogue **Imprimer** , sélectionnez l’imprimante et les paramètres que vous souhaitez utiliser, puis cliquez sur **OK**.  
  
     Pour modifier les paramètres par défaut, cliquez sur le bouton **Propriétés** . La taille de la page est déterminée par la hauteur et la largeur par défaut de la taille de la page de rapport, conformément à la définition de rapport. Les possibilités de modification des dimensions des pages dépendent des fonctionnalités de l'imprimante utilisée.  
  
     Pour afficher le rapport avant de l’imprimer, cliquez sur le bouton **Aperçu** . Cela entraîne l’ouverture de la première page du rapport dans une fenêtre d’aperçu distincte. Les pages supplémentaires sont disponibles lors du rendu du rapport sur le serveur de rapports. L'aperçu d'un rapport est rendu au format EMF. Vous pouvez accéder aux pages précédentes ou suivantes tant que la dernière page n'est pas atteinte et que le bouton **Suivant** n'est pas désactivé. Pour modifier les marges d’impression de la page d’aperçu, cliquez sur le bouton **Marges** . La boîte de dialogue **Marges** s’affiche. Configurez les marges supérieure, inférieure, gauche et droite selon vos besoins, puis cliquez sur **OK**. La boîte de dialogue se ferme et les paramètres sont stockés pour l'aperçu ainsi que l'impression du rendu.  
  
## <a name="see-also"></a> Voir aussi  
 [Activer et désactiver l'impression côté client pour Reporting Services](../../reporting-services/report-server/enable-and-disable-client-side-printing-for-reporting-services.md)  
  
  
