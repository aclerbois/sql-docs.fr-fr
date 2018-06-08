---
title: Imprimer des rapports à partir d’un navigateur à l’aide du contrôle d’impression (Générateur de rapports et SSRS) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-builder
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 10054250-d915-4bcb-8a1d-26837db4e932
caps.latest.revision: 13
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: f96460a280bd11f59ffd0e042eefbbc9be0b0188
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="print-reports-from-a-browser-with-the-print-control-report-builder-and-ssrs"></a>Imprimer des rapports à partir d'un navigateur à l'aide du contrôle d'impression (Générateur de rapports et SSRS)
  Bien qu'un navigateur soit l'application cliente la plus fréquemment utilisée pour afficher un rapport, la fonctionnalité d'impression du navigateur n'est pas le meilleur choix pour l'impression des rapports. La fonctionnalité d'impression d'un navigateur est conçue pour l'impression des pages Web. En règle générale, les pages que vous imprimez à partir d'un navigateur comprennent tous les éléments visuels présents sur une page Web, ainsi que des informations d'en-tête et de pied de page qui identifient la page ou le site Web. L'impression à partir d'un navigateur permet d'imprimer le contenu de la fenêtre actuelle. Pour un rapport de plusieurs pages, le navigateur imprime au mieux la première page et même moins encore, si la page de rapport dépasse les dimensions d'une page imprimée.  
  
 Pour améliorer la qualité d’impression des rapports affichés dans un navigateur et pour imprimer plusieurs pages, vous pouvez utiliser la fonctionnalité d’impression côté client de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. L’impression côté client permet d’accéder à une boîte de dialogue **Imprimer** standard dans laquelle vous pouvez sélectionner une imprimante, définir les propriétés des pages et des marges, et afficher l’aperçu du rapport avant son impression. L’impression côté client est destinée à être utilisée à la place de la commande **Imprimer** du menu **Fichier** du navigateur. Lorsque vous utilisez l'impression côté client, le rapport est imprimé tel qu'il a été créé, sans les éléments supplémentaires visibles lors de l'impression d'une page Web.  
  
 Pour utiliser l'impression côté client, vous devez installer un contrôle ActiveX [!INCLUDE[msCoName](../../includes/msconame-md.md)] . Pour plus d’informations, consultez [Activer et désactiver l’impression côté client pour Reporting Services](../../reporting-services/report-server/enable-and-disable-client-side-printing-for-reporting-services.md).  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
## <a name="print-options"></a>Options d'impression  
 Pour configurer les propriétés d'impression de votre rapport, cliquez dans la boîte de dialogue **Imprimer** sur le bouton **Propriétés** . Le**format de papier** est déterminé par la hauteur et la largeur par défaut de la taille de la page de rapport, conformément à la définition de rapport. Les valeurs disponibles dépendent du type d'imprimante et des fonctionnalités de cette dernière. La largeur et la hauteur indiquent les valeurs par défaut définies par les pilotes d'impression configurés sur l'ordinateur. La modification de ces valeurs permet d'imprimer le rapport selon de nouvelles dimensions. La largeur et la hauteur de page sont déterminées chacune par l' **orientation**, qui a pour valeur **Portrait** ou **Paysage**. L'orientation par défaut affichée dépend de la largeur et de la hauteur de page du rapport.  
  
> [!NOTE]  
>  La boîte de dialogue **Imprimer** et les paramètres d’impression par défaut pour la largeur, la hauteur et l’orientation de la page sont déterminés par la définition de rapport.  
  
### <a name="print-preview"></a>Aperçu avant impression  
 Pour afficher l'aperçu avant impression d'un rapport, cliquez dans la boîte de dialogue **Imprimer** sur le bouton **Aperçu** . Cela entraîne l'ouverture de la première page du rapport dans une fenêtre d'aperçu distincte. Les pages supplémentaires sont disponibles lors du rendu du rapport sur le serveur de rapports. L'aperçu d'un rapport est rendu au format EMF. Vous pouvez accéder aux pages précédentes ou suivantes tant que la dernière page n'est pas atteinte et que le bouton **Suivant** n'est pas désactivé.  
  
### <a name="adjusting-print-margins"></a>Réglage des marges d'impression  
 Vous pouvez modifier les marges d'impression dans le rendu du rapport EMF avant l'impression de ce dernier. Pour ce faire, cliquez dans la boîte de dialogue **Imprimer** sur le bouton **Aperçu** . En haut de la page d'aperçu, cliquez sur le bouton **Marges** . La boîte de dialogue Marges s'affiche. Configurez la marge supérieure, inférieure, de gauche et de droite selon vos besoins. [!INCLUDE[clickOK](../../includes/clickok-md.md)] La boîte de dialogue se ferme et les paramètres sont stockés pour l'aperçu ainsi que l'impression du rendu.  
  
## <a name="see-also"></a> Voir aussi  
 [Imprimer des rapports &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-builder/print-reports-report-builder-and-ssrs.md)   
 [Imprimer un rapport &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-builder/print-a-report-report-builder-and-ssrs.md)  
  
  
