---
title: Parties de rapports et datasets dans le Générateur de rapports | Microsoft Docs
ms.custom: ''
ms.date: 09/16/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-data
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 1fe86481-9c41-4535-a4b7-c7c4d780cab6
caps.latest.revision: 9
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 2e5d0ee295fc34fbb0d319e9354cb2a5b6658e06
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="report-parts-and-datasets-in-report-builder"></a>Parties de rapports et datasets dans le Générateur de rapports
  Dans le Générateur de rapports, la façon la plus simple d'inclure des données dans un rapport est d'ajouter des parties de rapports à partir de la bibliothèque de parties de rapports. Les parties de rapports contiennent les datasets desquels elles dépendent, appelés *datasets dépendants*. Les datasets dépendants sont basés sur les sources de données partagées et peuvent être des datasets incorporés ou partagés. En savoir plus sur les [Parties de rapports](../../reporting-services/report-design/report-parts-report-builder-and-ssrs.md).  
  
 Une autre méthode tout aussi simple pour inclure des données dans un rapport consiste à utiliser un dataset partagé. Pour plus d’informations, consultez [Datasets incorporés dans le rapport et datasets partagés &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/report-embedded-datasets-and-shared-datasets-report-builder-and-ssrs.md).  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
##  <a name="Adding"></a> Ajout d'une partie de rapport avec les datasets dépendants à votre rapport  
 Lorsque vous ajoutez une partie de rapport à votre rapport, les datasets dépendants qu'elle contient sont également ajoutés à votre rapport. Étant donné qu'une partie de rapport peut inclure un rectangle qui contient de nombreux autres éléments de rapport, elle peut ajouter plusieurs datasets dépendants à votre rapport. Chaque dataset partagé est une référence indépendante ; la source de données partagée de laquelle il dépend n'est pas ajoutée à votre rapport. Chaque dataset incorporé ajoute également la source de données incorporée ou partagée de laquelle il dépend.  
  
 Les informations d'identification pour une source de données incorporée ne sont pas enregistrées dans le cadre de la partie de rapport. Si une source de données incorporée est ajoutée à votre rapport, vous êtes invité à entrer des informations d'identification lorsque vous exécutez le rapport. Pour éviter d'être invité à entrer des informations d'identification, utilisez les parties de rapport basées sur les sources de données partagées avec des informations d'identification stockées.  
  
 Une fois que vous avez ajouté une partie de rapport à votre rapport, les datasets ajoutés ne sont pas différents des datasets incorporés ou partagés que vous créez. Vous pouvez consulter les datasets supplémentaires dans le volet des données de rapport. Les datasets incorporés s'affichent sous la source de données partagée correspondante et les datasets partagés s'affichent sous le dossier Datasets partagés.  
  
##  <a name="Customizing"></a> Personnalisation des datasets dépendants  
 Après avoir ajouté des parties de rapport à votre rapport, vous pouvez en afficher un aperçu et décider d'apporter des modifications aux données. Les modifications possibles dépendent du type de dataset que vous utilisez.  
  
 Pour modifier les données et options de données pour un dataset incorporé, vous pouvez modifier les propriétés de dataset, notamment la requête, comme si vous aviez créé le dataset vous-même.  
  
 Pour modifier les données et options de données pour un dataset partagé, vous pouvez modifier la définition de dataset partagé sur le serveur de rapports uniquement si vous avez des autorisations suffisantes. Vous pouvez également personnaliser l'instance du dataset partagé dans votre rapport en ajoutant des filtres, en ajoutant des champs calculés et en modifiant les options de données, telles que le respect de la casse. Pour plus d’informations, consultez [Datasets incorporés et partagés &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/embedded-and-shared-datasets-report-builder-and-ssrs.md).  
  
 Pour plus d’informations sur la modification de la définition d’un dataset partagé ou sur l’affichage des dernières modifications des données d’un dataset partagé dans votre rapport, consultez [Créer un dataset partagé ou incorporé &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/create-a-shared-dataset-or-embedded-dataset-report-builder-and-ssrs.md) et [Ajouter, modifier ou actualiser des champs dans le volet des données de rapport &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/add-edit-refresh-fields-in-the-report-data-pane-report-builder-and-ssrs.md).  
  
##  <a name="Publishing"></a> Publication des datasets dépendants comme datasets partagés  
 Lorsque vous publiez un élément de rapport qui a des datasets dépendants, vous avez la possibilité de publier chaque dataset comme un dataset partagé ou comme un dataset incorporé qui reste incorporé dans l'élément de rapport.  
  
 Lorsque vous sélectionnez l'option de dataset partagé, le dataset est enregistré sur le serveur de rapports en tant que définition de dataset partagé. Dans votre rapport, chaque élément de rapport qui utilise ce dataset est mis à jour pour pointer vers le dataset partagé qui est maintenant sur le serveur de rapports. Deux conséquences :  
  
1.  Dans la boîte de dialogue Publier, chaque dataset partagé qui a été publié est supprimé de la liste des éléments qui sont disponibles pour la publication.  
  
2.  Lorsque vous quittez le Générateur de rapports ou démarrez un nouveau rapport, vous êtes invité à enregistrer votre rapport. Si vous n'enregistrez pas votre rapport, la prochaine fois que vous ouvrez ce rapport et publiez des éléments de rapport, vous pouvez publier de nouvelles copies des mêmes datasets. Pour éviter d'enregistrer plusieurs copies de datasets partagés sur le serveur de rapports, nous vous recommandons d'enregistrer le rapport.  
  
> [!IMPORTANT]  
>  Pour garantir que vous et d'autres utilisateurs pouvez continuer à utiliser avec succès des données d'un dataset partagé, vous devez comprendre les principes qui sous-tendent la sécurisation des éléments de rapport. Pour plus d’informations, consultez [Sécuriser les éléments de dataset partagés](../../reporting-services/security/secure-shared-dataset-items.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Vue Conception de rapport &#40;Générateur de rapports&#41;](../../reporting-services/report-builder/report-design-view-report-builder.md)   
 [Sécurité &#40;Générateur de rapports&#41;](../../reporting-services/report-builder/security-report-builder.md)   
 [Publication de parties de rapports &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/report-parts-report-builder-and-ssrs.md)   
 [Datasets incorporés dans le rapport et datasets partagés &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/report-embedded-datasets-and-shared-datasets-report-builder-and-ssrs.md)  
  
  
