---
title: Configurer la taille de téléchargement de fichier maximale (PowerPivot pour SharePoint) | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: ppvt-sharepoint
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 15ce366acc4244db16f0b58bbdc8226e9327416c
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="configure-maximum-file-upload-size-power-pivot-for-sharepoint"></a>Configurer la taille maximale de téléchargement de fichiers (PowerPivot pour SharePoint)
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] Les classeurs contiennent souvent des quantités importantes de données qui se traduisent par des fichiers dépassant la taille de fichier maximale autorisée pour les chargements SharePoint. Lorsque vous essayez de télécharger un fichier qui dépasse la limite supérieure, l'erreur suivante s'affiche dans SharePoint :  
  
-   « La taille du fichier spécifié est supérieure à la taille maximale de fichier prise en charge. »  
  
 Pour augmenter la taille de fichier, commencez par ajuster la valeur de l'option Taille maximale du classeur pour Excel Services à 50 mégaoctets. Cela aligne les limites de taille de fichier maximale pour Excel sur celles des applications Web SharePoint (définies par défaut sur 50 mégaoctets dans SharePoint 2010, et sur 200 dans SharePoint 2013). Si la taille de vos fichiers est supérieure à 50 mégaoctets, augmentez les limites de taille de fichier pour Excel Services ainsi que pour votre application Web.  
  
 Vous devez être administrateur SharePoint pour modifier la taille maximale du téléchargement de fichiers.  
  
### <a name="configure-maximum-file-size-for-excel-services"></a>Configurer la taille de fichier maximale pour Excel Services  
  
1.  Dans l'Administration centrale, sous Gestion des applications, cliquez sur **Gérer les applications de service**.  
  
2.  Cliquez sur le nom de votre application Excel Services.  
  
3.  Cliquez sur **Emplacements de fichiers approuvés**.  
  
4.  Cliquez sur l'emplacement pour modifier les propriétés. Par défaut, Excel Services considère l'application Web par défaut comme un site de confiance. Si vous utilisez l’application web par défaut, cliquez sur **http://** pour ouvrir la page de configuration de cet emplacement.  
  
5.  Faites défiler la liste jusqu’à **Propriétés du classeur**.  
  
6.  Dans Taille maximale du classeur, faites passer la taille de fichier de 10 (valeur par défaut) à 50 ou à une taille plus importante adaptée aux fichiers que vous utilisez.  
  
     Par défaut, la taille maximale du téléchargement de fichiers pour les applications Web SharePoint est de 50 mégaoctets. Si vous affectez à l'option Taille maximale du classeur une valeur supérieure à 50 mégaoctets, suivez les étapes de la procédure suivante pour affecter la même valeur à l'option Taille maximale du téléchargement pour votre application Web SharePoint.  
  
     La valeur maximale que vous pouvez spécifier est 2 gigaoctets (ou 2 047 mégaoctets tel que le spécifie l'Administration centrale).  
  
7.  Cliquez sur **OK**.  
  
### <a name="configure-maximum-file-size-for-a-sharepoint-web-application"></a>Configurer la taille de fichier maximale pour une application Web SharePoint  
  
1.  Dans Administration Centrale, sous Gestion des applications, cliquez sur **Gérer les applications Web**.  
  
    > [!NOTE]  
    >  Effectuez les étapes suivantes uniquement si vous augmentez la Taille maximale du classeur dans Excel Services.  
  
2.  Sélectionnez l’application (par exemple, **SharePoint - 80**).  
  
3.  Dans le ruban Applications Web, cliquez sur la flèche bas située sur le bouton Paramètres généraux.  
  
4.  Cliquez sur **Paramètres généraux**.  
  
5.  Faites défiler la liste jusqu’à **Taille maximale du téléchargement**.  
  
6.  Affectez à la propriété la même taille, ou une taille supérieure à la Taille maximale du classeur dans Excel Services.  
  
7.  Cliquez sur **OK**.  
  
  
