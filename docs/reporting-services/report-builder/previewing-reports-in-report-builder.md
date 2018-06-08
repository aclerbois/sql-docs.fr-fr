---
title: Aperçu des rapports dans le Générateur de rapports | Microsoft Docs
ms.custom: ''
ms.date: 01/09/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-builder
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: ba6b5bdd-d8c6-4aa8-ba32-3a10b11969d4
caps.latest.revision: 8
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: eb6447d896a68a932e24223944fd6117f54638eb
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="previewing-reports-in-report-builder"></a>Aperçu des rapports dans le Générateur de rapports
  Quand vous créez un rapport paginé [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , il est utile d’afficher fréquemment un aperçu du rapport pour vérifier qu’il s’affiche comme vous le souhaitez. Cliquez sur **Exécuter**pour afficher un aperçu du rapport. Le rapport est restitué en mode Aperçu.  
  
 Le Générateur de rapports améliore l'expérience des utilisateurs en termes d'affichage de l'aperçu en utilisant des sessions d'édition lorsqu'il est connecté à un serveur de rapports. La session d'édition crée un cache de données et rend les datasets du cache disponibles pour afficher régulièrement un aperçu du rapport. Une session d'édition n'est pas une fonctionnalité avec laquelle vous interagissez directement, mais la compréhension du mécanisme d'actualisation du dataset mis en cache vous aidera à améliorer les performances de rendu du rapport lors de son affichage.  
  
 D'autres avantages des sessions d'édition sont la capacité de modifier des rapports qui utilisent des sources de données incorporées ou de faire référence à des éléments tels que des images ou des sous-rapports stockés sur le serveur de rapports.  
  
> [!NOTE]  
> Il existe des différences entre l’aperçu dans le Générateur de rapports et l’affichage dans un navigateur. Par exemple, un contrôle de calendrier, qui est ajouté à un rapport lorsque vous spécifiez un paramètre de type Date/heure, est différent dans le Générateur de rapports et dans un navigateur. 
  
## <a name="improving-preview-performance"></a>Amélioration de la performance d'aperçu  
 Le mode de création et de mise à jour d'un rapport affecte la rapidité de rendu de l'aperçu du rapport. La première fois que vous affichez un aperçu d'un rapport qui repose sur une référence de serveur, une session d'édition est créée pour vous et les données utilisées lorsque le rapport est exécuté sont ajoutées à un cache de données stocké sur le serveur de rapports. Lorsque vous apportez des modifications au rapport qui n'affectent pas les données, la copie mise en cache des données est utilisée par le rapport. Cela signifie que vous ne verrez pas vos données changer chaque fois que vous afficherez un aperçu du rapport. Si vous souhaitez afficher les nouvelles données, cliquez sur le bouton **Actualiser** sur le ruban.  
  
 Les actions suivantes provoquent l'actualisation du cache et ralentiront le rendu lors du prochain affichage de l'aperçu du rapport :  
  
-   Ajoutez, modifiez ou supprimez un dataset. Le dataset mis en cache contient tous les datasets utilisés par un rapport et toute modification apportée à un dataset rendra invalide le dataset mis en cache. Cela inclut la modification du nom, d'une requête ou de champs dans le dataset.  
  
    > [!NOTE]  
    >  Si le dataset contient un grand nombre des champs que vous ne prévoyez pas d'utiliser, envisagez de le mettre à jour pour omettre ces champs. Bien que la mise à jour crée une nouvelle session d'édition et ralentisse le premier aperçu du rapport, l'utilisation d'un dataset mis en cache plus petit améliore en général les performances du serveur de rapports.  
  
-   Ajoutez, modifiez ou supprimez une source de données. Cela inclut modifier le nom ou les propriétés de la source de données, l'extension de données de la source de données ou les propriétés de la connexion à la source de données.  
  
-   Modifiez la source de données partagée que le rapport utilise en une source de données différente.  
  
-   Modifiez la langue du rapport.  
  
-   Modifiez les assemblys ou le code personnalisé que le rapport utilise.  
  
-   Ajoutez, modifiez ou supprimez les paramètres de requête dans le rapport ou les valeurs de paramètres.  
  
 Les modifications de la mise en page de rapport et de la mise en forme des données n'affectent pas le dataset mis en cache. Vous pouvez effectuer les actions suivantes sans actualiser le dataset mis en cache :  
  
-   Ajoutez ou supprimez des régions de données telles que des tables, des matrices ou des graphiques.  
  
-   Ajoutez ou supprimez des colonnes du rapport. Tous les champs du dataset sont disponibles pour être utilisés dans le rapport. L'ajout ou la suppression de champs dans le rapport n'a aucun effet sur le dataset.  
  
-   Modifiez l'ordre des champs dans les tables et les matrices.  
  
-   Ajoutez, modifiez ou supprimez des groupes de lignes et de colonnes.  
  
-   Ajoutez, modifiez ou supprimez la mise en forme des valeurs de données dans les champs.  
  
-   Ajoutez, modifiez ou supprimez des images, des lignes ou des zones de texte.  
  
-   Modifiez les sauts de page.  
  
 La session d'édition est créée la première fois que vous affichez l'aperçu d'un rapport. Par défaut, une session d'édition dure 7 200 secondes (2 heures). La session est réinitialisée à deux heures chaque fois que vous exécutez le rapport. Lorsque la session d'édition expire, le cache de données est supprimé. Si la session d'édition expire, une autre session est créée automatiquement la prochaine fois que vous affichez un aperçu du rapport. La durée d'expiration des sessions d'édition est configurable. Si la valeur par défaut de deux heures est trop longue ou trop courte pour vos besoins, contactez l'administrateur du serveur de rapports.  
  
 Par défaut, le cache de données peut contenir jusqu'à cinq datasets. Si vous utilisez de nombreuses combinaisons différentes de valeurs de paramètres, le rapport peut nécessiter plus de données. Ceci requiert l'actualisation du cache et le rendu du rapport sera plus lent lors du prochain aperçu. Le nombre d'entrées dans le cache est configurable par l'administrateur du serveur de rapports.  
  
## <a name="concurrency-of-report-updates"></a>Concurrence de mises à jour de rapport  
 Fréquemment, l'affichage d'un aperçu d'un rapport constitue une étape de la mise à jour avant l'enregistrement du rapport sur un serveur de rapports. Lorsque vous mettez à jour un rapport, il est possible qu'une autre personne le mette à jour et l'enregistre au même moment. Le rapport enregistré en dernier représente la version de rapport qu'il est possible d'afficher et de mettre à jour ultérieurement. Cela signifie que la version du rapport dont vous avez affiché l'aperçu n'est peut-être pas la version que vous rouvrez. Vous disposez de l'option d'enregistrer le rapport sous un nouveau nom à l'aide de l'option **Enregistrer sous** du menu du Générateur de rapports.  
  
## <a name="external-report-items"></a>Éléments de rapport externes  
 Votre rapport peut inclure des éléments tels que des sources de données partagées, des images externes et des sous-rapports stockés séparément du rapport. Étant donné que les éléments sont stockés séparément, il est possible qu'ils soient déplacés vers un emplacement différent du serveur de rapports ou supprimés. Si cela se produit, il est possible que vous ne puissiez pas afficher un aperçu de votre rapport. Vous pouvez soit mettre à jour le rapport pour indiquer l'emplacement mis à jour de l'élément ou, si l'élément a été supprimé, le remplacer par un élément existant, soit supprimer la référence à l'élément dans le rapport.  
  
 Si un sous-rapport utilisé par votre rapport est modifié après la création de votre session d'édition, le rendu du rapport échouera dans l'aperçu. Pour afficher un aperçu du rapport avec succès, vous devez enregistrer le rapport ou cliquer sur **Actualiser** pour obtenir de nouvelles données.  
  
## <a name="see-also"></a> Voir aussi  
 [Datasets de rapport &#40;SSRS&#41;](../../reporting-services/report-data/report-datasets-ssrs.md)   
 [Mise en forme des éléments de rapport &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/formatting-report-items-report-builder-and-ssrs.md)   
 [Tables, matrices et listes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/tables-matrices-and-lists-report-builder-and-ssrs.md)   
 [Graphiques &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/charts-report-builder-and-ssrs.md)   
 [Tables, matrices et listes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/tables-matrices-and-lists-report-builder-and-ssrs.md)   
 [Enregistrement des rapports &#40;Générateur de rapports&#41;](../../reporting-services/report-builder/saving-reports-report-builder.md)  
  
  
