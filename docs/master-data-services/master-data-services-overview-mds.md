---
title: Vue d’ensemble de Master Data Services (MDS) | Microsoft Docs
ms.custom: ''
ms.date: 02/14/2017
ms.prod: sql
ms.prod_service: mds
ms.component: non-specific
ms.reviewer: ''
ms.suite: sql
ms.technology:
- master-data-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
keywords:
- what is master data
helpviewer_keywords:
- Master Data Services, overview
- Master Data Services
ms.assetid: 8a4c28b1-6061-4850-80b6-132438b8c156
caps.latest.revision: 28
author: leolimsft
ms.author: lle
manager: craigg
ms.openlocfilehash: a329416c51580e32fef969a48c96dfa6a68b1cb7
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="master-data-services-overview-mds"></a>Vue d’ensemble de Master Data Services (MDS)

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

Cette rubrique décrit les fonctionnalités d’organisation et de gestion des données clés de [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]. 
  
 [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] vous permet de gérer un ensemble principal de données de votre organisation. Vous pouvez organiser les données en modèles, créer des règles de mise à jour des données et contrôler qui met les données à jour. Avec Excel, vous pouvez partager le jeu de données principal avec d’autres personnes de votre organisation. 
  
 >  Pour obtenir une description de l’architecture [!INCLUDE[ssMDSshort_md](../includes/ssmdsshort-md.md)] , consultez l’article [Master Data Services -- The Basics](https://www.simple-talk.com/sql/database-delivery/master-data-services-basics) sur simple-talk.com. Pour plus d’informations sur les nouvelles fonctionnalités de [!INCLUDE[ssCurrent](../includes/sscurrent-md.md)], consultez [Nouveautés de Master Data Services &#40;MDS&#41;](../master-data-services/what-s-new-in-master-data-services-mds.md).  
   **Pour obtenir des instructions sur l’installation de [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], la configuration de la base de données et du site web, et le déploiement des exemples de modèles, consultez** [Installation et configuration de Master Data Services](../master-data-services/master-data-services-installation-and-configuration.md).  
  
 Dans [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)], le modèle est le conteneur de niveau le plus élevé dans la structure de vos données de référence. Vous créez un modèle pour gérer des groupes de données semblables, par exemple pour gérer des données de produits en ligne. Un modèle contient une ou plusieurs entités, et les entités contiennent des membres qui sont des enregistrements de données. Une entité est semblable à une table.  
  
 Par exemple, votre modèle de produit en ligne peut contenir des entités telles qu’un produit, une couleur et un style. L’entité de couleur peut contenir des membres pour les couleurs rouge, argent et noir.  
  
 ![Entité de couleur](../master-data-services/media/mds-productmodel-colorentity-composite.png "Entité de couleur")  
  
 Les modèles contiennent également les attributs qui sont définis dans des entités. Un attribut contient des valeurs qui décrivent les membres d’entité. Il existe des attributs de forme libre et des attributs basés sur un domaine.  Un attribut basé sur un domaine contient des valeurs qui sont remplies par les membres d’une entité et peuvent être utilisés en tant que valeurs d’attribut pour d’autres entités.  
  
 Par exemple, une entité de produit peut avoir des attributs de forme libre pour le coût et le poids. En outre, il existe un attribut basé sur un domaine pour la couleur ![Numéro 1](../master-data-services/media/mds-number1.png "Numéro 1") qui contient des valeurs qui sont remplies par les membres de l’entité de couleur. Cette liste principale de couleurs est utilisée comme valeurs d’attribut pour l’entité de produit ![Numéro 2](../master-data-services/media/mds-number2.png "Numéro 2").  
  
 ![Attribut basé sur un domaine pour la couleur](../master-data-services/media/mds-productentity-color-domainattribute.png "Attribut basé sur un domaine pour la couleur")  
  
 Les hiérarchies dérivées proviennent des relations entre les entités d’un modèle. Il s’agit de relations d’attributs basés sur un domaine. Dans le modèle de produit, par exemple, vous pouvez avoir une hiérarchie dérivée de couleurs ![Numéro 1](../master-data-services/media/mds-number1.png "Numéro 1") provenant de la relation entre les entités de couleur ![Numéro 2](../master-data-services/media/mds-number2.png "Numéro 2") et de produit ![Numéro 3](../master-data-services/media/mds-number3.png "Numéro 3").  
  
 ![Hiérarchie dérivée de couleurs](../master-data-services/media/mds-derivedhierarchy.png "Hiérarchie dérivée de couleurs")  
  
 Une fois que vous avez défini une structure de base pour vos données, vous pouvez commencer à ajouter des enregistrements de données (membres) à l’aide de la fonctionnalité d’importation. Vous chargez les données dans des tables de mise en lots, vous validez les données à l’aide de règles d’entreprise, puis vous chargez les données dans des tables MDS.  Vous pouvez également utiliser des règles d’entreprise pour définir des valeurs d’attribut.  
  
 Le tableau suivant présente les principales tâches [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] . Sauf indication contraire, pour effectuer l'ensemble des procédures qui suivent, vous devez être un administrateur de modèle. Pour plus d’informations, consultez [Administrateurs &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md).  
  
> [!NOTE]  
>  Vous pouvez compléter les tâches suivantes dans un environnement de test et utiliser les exemples de données fournis lorsque vous installez [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]. Pour plus d’informations, consultez [Déploiement de modèles &#40;Master Data Services&#41;](../master-data-services/deploying-models-master-data-services.md).  
  
|Action|Détails|Rubriques connexes|  
|------------|-------------|--------------------|  
|Créer un modèle|Lorsque vous créez un modèle, il est considéré comme étant la VERSION_1.|[Modèles &#40;Master Data Services&#41;](../master-data-services/models-master-data-services.md)<br /><br /> [Créer un modèle &#40;Master Data Services&#41;](../master-data-services/create-a-model-master-data-services.md)|  
|Créer des entités|Créez autant d'entités que nécessaire pour contenir vos membres.|[Entités &#40;Master Data Services&#41;](../master-data-services/entities-master-data-services.md)<br /><br /> [Créer une entité &#40;Master Data Services&#41;](../master-data-services/create-an-entity-master-data-services.md)|  
|Créer des entités à utiliser comme attributs basés sur un domaine|Pour créer un attribut basé sur un domaine, commencez par créer l'entité pour remplir la liste des valeurs d'attribut.|[Attributs basés sur un domaine &#40;Master Data Services&#41;](../master-data-services/domain-based-attributes-master-data-services.md)<br /><br /> [Créer un attribut basé sur un domaine &#40;Master Data Services&#41;](../master-data-services/create-a-domain-based-attribute-master-data-services.md)|  
|Créer des attributs pour vos entités|Créez des attributs pour décrire des membres. Les attributs de nom et de code sont inclus automatiquement dans chaque entité et ne peuvent pas être supprimés. Vous pouvez créer d'autres attributs de forme libre pour contenir du texte, des dates, des nombres ou des fichiers.|[Attributs &#40;Master Data Services&#41;](../master-data-services/attributes-master-data-services.md)<br /><br /> [Créer un attribut de texte &#40;Master Data Services&#41;](../master-data-services/create-a-text-attribute-master-data-services.md)<br /><br /> [Créer un attribut numérique &#40;Master Data Services&#41;](../master-data-services/create-a-numeric-attribute-master-data-services.md)<br /><br /> [Créer un attribut de date &#40;Master Data Services&#41;](../master-data-services/create-a-date-attribute-master-data-services.md)<br /><br /> [Créer un attribut de lien &#40;Master Data Services&#41;](../master-data-services/create-a-link-attribute-master-data-services.md)<br /><br /> [Créer un attribut de fichier &#40;Master Data Services&#41;](../master-data-services/create-a-file-attribute-master-data-services.md)|  
|Créer des groupes d'attributs|Si vous avez plus de quatre ou cinq attributs pour une entité, vous pouvez créer des groupes d'attributs. Ces groupes correspondent aux onglets affichés au-dessus de la grille dans l' **Explorateur** et permettent de faciliter la navigation en regroupant les attributs.|[Groupes d’attributs &#40;Master Data Services&#41;](../master-data-services/attribute-groups-master-data-services.md)<br /><br /> [Créer un groupe d’attributs &#40;Master Data Services&#41;](../master-data-services/create-an-attribute-group-master-data-services.md)|  
|Importer des membres pour vos entités de prise en charge|Importez les données de vos entités de prise en charge à l’aide du processus de mise en lots. Pour le modèle Product, cela peut signifier l'importation des couleurs ou des tailles. Vous pouvez également créer les membres manuellement.<br /><br /> <br /><br /> Remarque : les utilisateurs peuvent créer des membres dans [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] s’ils ont au minimum l’autorisation **Mise à jour** sur un objet modèle feuille d’une entité et accès à la zone fonctionnelle de l’ **Explorateur** .|[Vue d’ensemble : importation de données à partir de tables &#40;Master Data Services&#41;](../master-data-services/overview-importing-data-from-tables-master-data-services.md)<br /><br /> [Créer un membre feuille &#40;Master Data Services&#41;](../master-data-services/create-a-leaf-member-master-data-services.md)|  
|Créer et appliquer des règles d’entreprise pour vérifier la qualité des données|Créez et publiez des règles d'entreprise pour garantir l'exactitude de vos données. Vous pouvez utiliser des règles d'entreprise pour :<br /><br /> Définir les valeurs d'attribut par défaut.<br /><br /> Modifier les valeurs d'attribut.<br /><br /> Envoyer des notifications par courrier électronique lorsque les données ne sont pas validées par une règle d'entreprise.|[Business Rules &#40;Master Data Services&#41;](../master-data-services/business-rules-master-data-services.md)<br /><br /> [Créer et publier une règle d’entreprise &#40;Master Data Services&#41;](../master-data-services/create-and-publish-a-business-rule-master-data-services.md)<br /><br /> [Valider des membres spécifiques par rapport aux règles d'entreprise (Master Data Services)](../master-data-services/validate-specific-members-against-business-rules-master-data-services.md)<br /><br /> [Configurer des notifications par courrier électronique &#40;Master Data Services&#41;](../master-data-services/configure-email-notifications-master-data-services.md)<br /><br /> [Configurer des règles d’entreprise pour envoyer des notifications &#40;Master Data Services&#41;](../master-data-services/configure-business-rules-to-send-notifications-master-data-services.md)|  
|Importer des membres de vos entités primaires et appliquer des règles d'entreprise|Importez les membres de vos entités primaires à l'aide du processus de site. Lorsque vous avez terminé, validez la version pour appliquer les règles d'entreprise à tous les membres dans la version de modèle.<br /><br /> Vous pouvez ensuite corriger tous les problèmes de la validation de la règle d'entreprise.|[Validation &#40;Master Data Services&#41;](../master-data-services/validation-master-data-services.md)<br /><br /> [Valider une version par rapport aux règles d’entreprise &#40;Master Data Services&#41;](../master-data-services/validate-a-version-against-business-rules-master-data-services.md)<br /><br /> [Procédure stockée de validation &#40;Master Data Services&#41;](../master-data-services/validation-stored-procedure-master-data-services.md)|  
|Créer des hiérarchies dérivées|Les hiérarchies dérivées peuvent être mises à jour en fonction de l’évolution des besoins de l’entreprise et garantissent que tous les membres sont pris en compte pour le niveau approprié.|[Hiérarchies dérivées &#40;Master Data Services&#41;](../master-data-services/derived-hierarchies-master-data-services.md)<br /><br /> [Créer une hiérarchie dérivée &#40;Master Data Services&#41;](../master-data-services/create-a-derived-hierarchy-master-data-services.md)|  
|Si nécessaire, créez des hiérarchies explicites|Si vous souhaitez créer des hiérarchies non basées sur le niveau et qui incluent des membres d'une entité unique, vous pouvez créer des hiérarchies explicites.|[Hiérarchies explicites &#40;Master Data Services&#41;](../master-data-services/explicit-hierarchies-master-data-services.md)<br /><br /> [Créer une hiérarchie explicite &#40;Master Data Services&#41;](../master-data-services/create-an-explicit-hierarchy-master-data-services.md)|  
|Si nécessaire, créez des collections|Créez une collection si vous souhaitez afficher des regroupements différents de vos membres pour la création de rapports ou l'analyse, et si vous n'avez pas besoin d'une hiérarchie complète.<br /><br /> <br /><br /> Remarque : les utilisateurs peuvent créer des collections dans [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)] s’ils ont au minimum l’autorisation **Mise à jour** sur l’objet modèle collection et accès à la zone fonctionnelle de l’ **Explorateur** .|[Collections &#40;Master Data Services&#41;](../master-data-services/collections-master-data-services.md)<br /><br /> [Créer une collection &#40;Master Data Services&#41;](../master-data-services/create-a-collection-master-data-services.md)|  
|Créer des métadonnées définies par l'utilisateur|Pour décrire vos objets modèle, ajoutez des métadonnées définies par l'utilisateur à votre modèle. Les métadonnées peuvent inclure le propriétaire d'un objet ou la source des données.||  
|Verrouiller une version de votre modèle et affecter un indicateur de version|Verrouillez une version de votre modèle pour empêcher la modification des membres, sauf par les administrateurs. Une fois que les données de la version ont été validées par les règles d'entreprise, vous pouvez valider la version et empêcher ainsi à tous les utilisateurs de modifier les membres.<br /><br /> Créez et affectez un indicateur de version au modèle. Les indicateurs aident les utilisateurs et systèmes d'abonnement à identifier la version d'un modèle à utiliser.|[Versions &#40;Master Data Services&#41;](../master-data-services/versions-master-data-services.md)<br /><br /> [Verrouiller une version &#40;Master Data Services&#41;](../master-data-services/lock-a-version-master-data-services.md)<br /><br /> [Créer un indicateur de version &#40;Master Data Services&#41;](../master-data-services/create-a-version-flag-master-data-services.md)|  
|Créer des vues d'abonnement|Pour que vos systèmes d'abonnement consomment vos données de référence, créez des vues d'abonnement qui créent des vues standard dans la base de données [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)] .|[Vue d’ensemble : exportation de données &#40;Master Data Services&#41;](../master-data-services/overview-exporting-data-master-data-services.md)<br /><br /> [Créer une vue d’abonnement pour exporter des données &#40;Master Data Services&#41;](../master-data-services/create-a-subscription-view-to-export-data-master-data-services.md)|  
|Configurer les autorisations d'accès|Vous ne pouvez pas copier les autorisations d'accès d'un environnement test vers un environnement de production. Toutefois, vous pouvez utiliser votre environnement de test pour déterminer la sécurité que vous souhaitez utiliser en production.|[Sécurité &#40;Master Data Services&#41;](../master-data-services/security-master-data-services.md)<br /><br /> [Ajouter un groupe &#40;Master Data Services&#41;](../master-data-services/add-a-group-master-data-services.md)<br /><br /> [Ajouter un utilisateur &#40;Master Data Services&#41;](../master-data-services/add-a-user-master-data-services.md)|  
  
 Lorsque vous serez prêt, vous pourrez déployer votre modèle, avec ou sans ses données, dans votre environnement de production. Pour plus d’informations, consultez [Déploiement de modèles &#40;Master Data Services&#41;](../master-data-services/deploying-models-master-data-services.md).  
  
  

