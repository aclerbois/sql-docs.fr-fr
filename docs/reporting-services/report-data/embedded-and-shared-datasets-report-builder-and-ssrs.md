---
title: Datasets incorporés et partagés (Générateur de rapports et SSRS) | Microsoft Docs
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
ms.assetid: adc95cc0-d15a-413d-bc5a-302eab37a069
caps.latest.revision: 7
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: d4cca525b1549ad37388f6c4f0c45676c750d0cd
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="embedded-and-shared-datasets-report-builder-and-ssrs"></a>Datasets incorporés et partagés (Générateur de rapports et SSRS)
  Dans un rapport, un dataset représente des données de rapport retournées comme résultat de l'exécution d'une requête sur une source de données externe. Le dataset dépend de la connexion de données qui contient des informations sur la source de données externe. Les données elles-mêmes ne sont pas intégrées dans la définition de rapport. Un dataset contient une commande de requête, une collection de champs, des paramètres, des filtres et des options de données incluant notamment le respect de la casse et le classement. Il existe deux types de datasets :  
  
-   **Datasets partagés.** Un dataset partagé est publié sur un serveur de rapports et peut être utilisé par plusieurs rapports. Un dataset partagé doit être basé sur une source de données partagée. Un dataset partagé peut être mis en cache et planifié en créant un plan d'actualisation du cache.  
  
-   **Datasets incorporés.** Les datasets incorporés sont définis dans un rapport unique et sont utilisés par un seul rapport.  
  
 La différence entre les deux réside dans leur mode de création, de stockage et de gestion.  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
## <a name="shared-datasets"></a>Datasets partagés  
 Utilisez un dataset partagé pour fournir une requête qui peut être utilisée par plusieurs rapports. Les datasets partagés sont stockés sur le serveur de rapports et gérés séparément des rapports ou des sources de données partagées. Par exemple, un administrateur de serveur de rapports peut mettre à jour la requête pour tirer parti de l'indexation améliorée ou d'une autre optimisation des performances des requêtes.  
  
 Nous vous recommandons d'utiliser des datasets partagés dans la mesure du possible. Vous pouvez optimiser une requête ou mettre en cache les résultats de la requête pour améliorer les performances des rapports. Les datasets partagés permettent de gérer plus facilement l'accès aux données, et de sécuriser davantage les rapports et les datasets auxquels ils accèdent et de les rendre plus performants.  
  
 Dans le Concepteur de rapports, vous pouvez créer des datasets partagés dans le cadre d'un projet de rapport, et contrôler s'il convient de les déployer sur un serveur de rapports. Vous ne pouvez pas rechercher et sélectionner un dataset partagé sur un serveur de rapports, et l'ajouter à votre rapport.  
  
 Dans le Générateur de rapports, vous pouvez effectuer les actions suivantes :  
  
1.  Pour créer un dataset partagé, utilisez le mode création de dataset partagé. Vous pouvez l'enregistrer sur un serveur de rapports ou un site SharePoint pour le partager avec d'autres rapports. Vous pouvez également accéder au serveur de rapports et modifier tout dataset partagé existant. Dans ce mode, vous pouvez créer une requête et définir toutes les options de dataset. Pour plus d’informations, consultez [Mode création de dataset partagé &#40;Générateur de rapports&#41;](../../reporting-services/report-builder/shared-dataset-design-view-report-builder.md).  
  
2.  Pour ajouter un dataset partagé à votre rapport, ouvrez le Générateur de rapports en mode création de rapport. Depuis un Assistant ou le volet Données du rapport, accédez au serveur de rapports et sélectionnez le dataset partagé à ajouter à votre rapport. Dans ce mode, vous ne pouvez pas modifier la requête sauf pour ajouter des champs. Vous pouvez remplacer d'autres options de données et ajouter des filtres. Vous ne pouvez pas supprimer de filtres.  
  
3.  Le tableau suivant compare les propriétés qui peuvent être configurées pour la définition du dataset partagé sur le serveur de rapports et l'instance du dataset partagé dans la définition de rapport.  
  
    |Propriété|Remarques sur la configuration pour la définition|Remarques sur la configuration pour l'instance|  
    |--------------|--------------------------------------------|------------------------------------------|  
    |Texte de la requête|Configurez la requête, notamment définissez-la en tant qu'expression.|Impossible de modifier la requête.|  
    |Paramètres de requête|Impossible de référencer des paramètres de rapport<br /><br /> Inclut des valeurs par défaut<br /><br /> Inclut un indicateur en lecture seule|Configurez les paramètres qui ne sont pas marqués en lecture seule dans la définition|  
    |Filtres|Définir les filtres|Impossible d'afficher ou modifier des filtres de dataset qui font partie de la définition<br /><br /> Possibilité de créer des filtres supplémentaires|  
    |Source de données|Doit être une source de données partagée|Impossible de modifier la source de données partagée|  
    |Champs|Champs de la commande de requête<br /><br /> Les champs calculés ne font pas partie de la définition de dataset|Possibilité de consulter les champs, mais pas de les modifier<br /><br /> La collection de champs est statique selon la requête au moment où vous avez ajouté le dataset partagé au rapport. Pour mettre à jour, cliquez sur **Actualiser les champs** dans la boîte de dialogue **Propriétés du dataset** . La collection de champs réelle est tout élément retourné par la requête actuelle dans la définition.<br /><br /> Ajouter des champs calculés|  
    |Dataset|Options de données telles que le respect de la casse|Remplacer des options de données dans l'instance|  
  
## <a name="embedded-datasets"></a>Datasets incorporés  
 Utilisez un dataset incorporé lorsque vous souhaitez obtenir des données d'une source de données externe à utiliser uniquement dans un rapport. Les datasets incorporés sont utiles lorsque vous souhaitez créer une requête qui n'a pas d'autres dépendances et que vous n'avez pas besoin d'utiliser pour plusieurs rapports.  
  
 Pour créer ou modifier un dataset incorporé, utilisez le volet des données de rapport. Après avoir créé un dataset, vous pouvez configurer les propriétés dans la boîte de dialogue **Propriétés du dataset** .  
  
## <a name="see-also"></a> Voir aussi  
 [Connexions de données ou sources de données incorporées et partagées &#40;Générateur de rapports et SSRS&#41;](http://msdn.microsoft.com/library/f417782c-b85a-4c4d-8a40-839176daba56)   
 [Créer un dataset partagé ou incorporé &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/create-a-shared-dataset-or-embedded-dataset-report-builder-and-ssrs.md)   
 [Jeux de données du rapport &#40;SSRS&#41;](../../reporting-services/report-data/report-datasets-ssrs.md)   
 [Collection de champs de dataset &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/dataset-fields-collection-report-builder-and-ssrs.md)   
 [Connexions de données, sources de données et chaînes de connexion dans le Générateur de rapports](http://msdn.microsoft.com/library/7e103637-4371-43d7-821c-d269c2cc1b34)   
 [Connexions de données, sources de données et chaînes de connexion &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md)  
  
  
