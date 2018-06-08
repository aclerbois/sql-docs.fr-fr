---
title: Ajouter le composant WebPart Visionneuse de rapports à une page web | Microsoft Docs
ms.custom: ''
ms.date: 10/05/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-server-sharepoint
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 5d9eb107d1355a15d15c22fc3b24824cae009075
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="add-the-report-viewer-web-part-to-a-web-page"></a>Ajouter le composant WebPart Visionneuse de rapports à une page web

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016](../../includes/ssrs-appliesto-2016.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-2016i](../../includes/ssrs-appliesto-sharepoint-2013-2016.md)] [!INCLUDE[ssrs-appliesto-not-pbirsi](../../includes/ssrs-appliesto-not-pbirs.md)])

[!INCLUDE [ssrs-previous-versions](../../includes/ssrs-previous-versions.md)]

Vous pouvez utiliser le composant WebPart Visionneuse de rapports pour afficher les rapports qui s’exécutent sur un serveur de rapports configuré en mode intégré SharePoint. Vous pouvez utiliser le composant WebPart pour afficher les fichiers de définition de rapport (.rdl) que vous avez créés dans le Générateur de rapports ou le Concepteur de rapports, et que vous avez téléchargés dans une bibliothèque.

Vous pouvez ajouter le composant WebPart Visionneuse de rapports à une page web si vous souhaitez incorporer un rapport dans cette page.

> [!NOTE]
> Cet article est spécifique au composant WebPart Visionneuse de rapports fourni avec le complément Reporting Services pour les produits SharePoint. L’intégration de Reporting Services à SharePoint n’est plus disponible après SQL Server 2016.

Pour ajouter un composant WebPart à une page web, vous devez disposer de l’autorisation Ajouter et personnaliser les pages au niveau du site. Si vous utilisez des paramètres de sécurité par défaut, cette autorisation est accordée aux membres du groupe **Propriétaires** qui ont le niveau d’autorisation Contrôle total.

## <a name="to-embed-a-report-in-a-web-page"></a>Pour incorporer un rapport dans une page web

1.  Ouvrez ou créez la page ou le tableau de bord de composant WebPart.  
  
2.  Dans **Actions de site**, cliquez sur **Modifier la page**.  
  
3.  Cliquez sur **Ajouter un composant WebPart**.  
  
4.  Dans la liste de catégories WebPart, sélectionnez la catégorie **Divers** , puis **Visionneuse de rapports SQL Server Reporting Services**.  
  
5.  Cliquez sur **Ajouter**. Le composant WebPart est ajouté en haut de la zone. Vous pouvez le faire glisser vers un autre emplacement de la zone.  
  
6.  Dans la visionneuse, cliquez sur **Cliquez ici pour ouvrir le volet d’outils**.  
  
7.  Sélectionnez un rapport dans une bibliothèque de la collection de sites actuelle en cliquant sur le bouton Parcourir (**...**). Vous pouvez également taper l'URL du rapport. Pour déterminer l’URL d’un rapport, cliquez avec le bouton droit sur le rapport, puis sélectionnez **Propriétés**. Ne cliquez pas sur la flèche orientée vers le bas en regard du rapport ; l'URL du rapport n'est pas indiquée dans la page Afficher les propriétés de l'élément. Si vous copiez et collez l’URL à partir de la boîte de dialogue **Propriétés** , remplacez l’encodage d’URL « %20 » par un espace (par exemple « Company%20Sales » doit s’écrire « Company Sales »).  
  
    > [!NOTE]  
    >  Chaque composant WebPart Visionneuse de rapports contient un seul rapport. L'URL doit correspondre au chemin d'accès complet d'un rapport situé sur le site SharePoint actuel ou sur un site inclus dans la même batterie de serveurs ou application Web. L'URL doit correspondre à une bibliothèque de documents ou à un dossier situé dans une bibliothèque de documents contenant le rapport. L'URL de rapport doit inclure l'extension de fichier .rdl. Si le rapport dépend d'un modèle ou de fichiers de source de données partagée, vous n'avez pas besoin de spécifier ces fichiers dans l'URL. Le rapport contient les références aux fichiers nécessaires.  
  
8.  Tant que le volet d'outils est ouvert, vous pouvez définir certaines propriétés afin de modifier l'apparence et la mise en page par défaut.  
  
9. Cliquez au bas du volet d’outils sur **Appliquer** , puis sur **OK** pour fermer le volet.  
  
## <a name="see-also"></a>Voir aussi

 [Composant WebPart Visionneuse de rapports sur un site SharePoint](../../reporting-services/report-server-sharepoint/report-viewer-web-part-on-a-sharepoint-site.md)   
 [Personnaliser le composant WebPart Visionneuse de rapports](../../reporting-services/report-server-sharepoint/customize-the-report-viewer-web-part.md)   
 [Accord d'autorisations sur des éléments de serveur de rapports sur un site SharePoint](../../reporting-services/security/granting-permissions-on-report-server-items-on-a-sharepoint-site.md)   
 [Installer ou désinstaller le complément Reporting Services pour SharePoint](../../reporting-services/install-windows/install-or-uninstall-the-reporting-services-add-in-for-sharepoint.md)  
