---
title: Créer un emplacement approuvé pour les sites Power Pivot dans l’Administration centrale | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: ppvt-sharepoint
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 24a779a751ac93c4c132c2dbf8ae63987de2666a
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="create-a-trusted-location-for-power-pivot-sites-in-central-administration"></a>Créer un emplacement approuvé pour les sites Power Pivot dans l’Administration centrale
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Excel Services vous permet de spécifier les emplacements qui constituent des référentiels valides pour les classeurs que vous ouvrez sur un serveur SharePoint. Ces emplacements sont appelés « emplacements approuvés ». Vous pouvez utiliser des paramètres de configuration différents pour chaque emplacement approuvé créé. Dans le cas d’un déploiement de [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour SharePoint, vous pouvez envisager de créer un emplacement approuvé pour les sites qui contiennent des classeurs [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] , afin d’appliquer les paramètres les mieux adaptés à l’accès aux données [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] tout en conservant les paramètres par défaut pour le reste de la batterie de serveurs.  
  
  
## <a name="prerequisites"></a>Configuration requise  
 Vous devez être administrateur de service ou de batterie de serveurs pour désigner une URL comme emplacement approuvé.  
  
 Vous devez connaître l’adresse URL du site SharePoint contenant la Galerie [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] ou une autre bibliothèque qui stocke vos classeurs. Pour obtenir cette adresse, ouvrez le site contenant la bibliothèque, cliquez avec le bouton droit sur **Galerie [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)]**, sélectionnez **Propriétés**, puis copiez la première partie de l’adresse (URL) qui contient le nom du serveur et le chemin d’accès au site.  
  
##  <a name="overview"></a> Vue d'ensemble  
 Une installation initiale d'Excel Services spécifie 'http://' comme son emplacement approuvé, ce qui signifie que les classeurs de n'importe quel site de la batterie peuvent être ouverts sur le serveur. Si vous avez besoin d'un contrôle plus strict sur les emplacements à considérer comme dignes de confiance, vous pouvez créer de nouveaux emplacements approuvés qui mappent à des sites spécifiques dans votre batterie, puis faire varier les paramètres et les autorisations pour chacun.  
  
 La création d’un emplacement approuvé pour les sites hébergeant des classeurs [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] est particulièrement utile si vous voulez conserver les valeurs par défaut pour le reste de la batterie de serveurs, tout en appliquant des paramètres différents mieux adaptés à l’accès aux données [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] . Par exemple, un emplacement approuvé optimisé pour les classeurs [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] peut disposer d’une taille de classeur maximale de 50 Mo, tandis que le reste de la batterie utilise la valeur par défaut de 10 Mo.  
  
 La création d’un emplacement approuvé est recommandée si vous utilisez des bibliothèques Galerie [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour afficher un aperçu des classeurs publiés, et que des avertissements d’actualisation des données s’affichent au lieu de l’image d’aperçu que vous attendez. [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] restitue des images miniatures des rapports et classeurs à l’aide de données et d’informations de présentation dans le document. Si l’avertissement lors de l’actualisation des données est activé pour un emplacement approuvé, il est possible que la Galerie [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] ne dispose pas d’autorisations suffisantes pour procéder à l’actualisation. Dans ce cas, une erreur est générée à la place de l’image miniature. L’ajout d’un site contenant la Galerie [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] comme nouvel emplacement approuvé permet d’éviter ce problème.  
  
##  <a name="create"></a> Créer un emplacement approuvé pour l’accès aux données Power Pivot  
  
1.  Dans l'Administration centrale, sous Gestion des applications, cliquez sur **Gérer les applications de service**.  
  
2.  Cliquez sur l'application de service Excel Services.  
  
3.  Cliquez sur **Emplacements de fichiers approuvés**.  
  
4.  Cliquez sur **Ajouter un emplacement de fichier approuvé**.  
  
5.  Entrez l’URL d’un site contenant une bibliothèque Galerie [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] .  
  
6.  Dans Type d'emplacement, sélectionnez **Microsoft SharePoint Foundation**.  
  
    > [!IMPORTANT]  
    >  Les types d’emplacements UNC et HTTP ne sont pas pris en charge pour l’accès aux données [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] .  
  
7.  Acceptez tous les paramètres par défaut pour les propriétés dans Gestion des sessions, Propriétés du classeur et Comportement du calcul.  
  
8.  Dans Propriétés du classeur, affectez à **Taille maximale du classeur** la valeur **50**. Cela aligne la limite supérieure de la taille des fichiers de classeurs sur la limite supérieure des téléchargements de fichiers vers l'application Web parente. Si la taille de vos classeurs est supérieure à 50 mégaoctets, vous devez augmenter encore plus la limite de la taille des fichiers. Pour plus d’informations, consultez [Configurer la taille maximale de chargement de fichiers &#40;PowerPivot pour SharePoint&#41;](../../analysis-services/power-pivot-sharepoint/configure-maximum-file-upload-size-power-pivot-for-sharepoint.md).  
  
9. Dans Données externes, vérifiez que l'option Autoriser les données externes a la valeur **Bibliothèques de connexions de données approuvées et incorporées**. Ce paramètre est obligatoire pour l’accès aux données [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] dans un classeur.  
  
10. De plus, dans Données externes, pour Avertir lors de l'actualisation, désactivez la case à cocher **Avertissement d'actualisation activé**. Le fait de décocher cette case permet à la Galerie [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] d’ignorer les messages d’avertissement habituels et d’afficher des images d’aperçu d’un classeur à la place.  
  
11. Cliquez sur **OK**.  
  
## <a name="see-also"></a>Voir aussi  
 [Galerie Power Pivot](http://msdn.microsoft.com/library/2a0db616-e08e-4062-aac8-979f8cad7794)   
 [Créer et personnaliser la galerie PowerPivot](../../analysis-services/power-pivot-sharepoint/create-and-customize-power-pivot-gallery.md)   
 [Utiliser la galerie Power Pivot](../../analysis-services/power-pivot-sharepoint/use-power-pivot-gallery.md)  
  
  
