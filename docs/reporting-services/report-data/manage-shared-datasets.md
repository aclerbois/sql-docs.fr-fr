---
title: Gérer des datasets partagés | Microsoft Docs
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
ms.assetid: 2cbb1fa3-959e-4df6-9887-ebc93cc1b686
caps.latest.revision: 7
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 7bd03c804791c67128d81eb3d7a6dbc64373e03b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="manage-shared-datasets"></a>Gérer des datasets partagés
  Dans [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], les datasets partagés récupèrent des données de sources de données partagées qui se connectent à des sources de données externes. Un dataset partagé offre un moyen de partager une requête pour fournir un jeu cohérent de données pour plusieurs rapports. La requête de dataset peut inclure des paramètres de dataset. Vous pouvez configurer un dataset partagé pour mettre en cache les résultats de la requête pour des combinaisons de paramètres spécifiques lors de la première utilisation ou en spécifiant une planification. Vous pouvez utiliser la mise en cache de datasets partagés en association avec la mise en cache de rapports et les sources de données de rapports pour mieux gérer l'accès à une source de données.  
  
 Les datasets partagés utilisent uniquement des sources de données partagées, pas des sources de données incorporées. Un dataset partagé peut être basé sur toute source de données pour une extension de données [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] prise en charge ou sur un modèle de rapport.  
  
## <a name="creating-and-using-shared-datasets"></a>Création et utilisation de datasets partagés  
 Pour créer un dataset partagé, vous devez utiliser une application qui crée un fichier de définition de dataset partagé (.rsd). Vous pouvez utiliser l'une des applications suivantes pour créer un dataset partagé :  
  
-   Générateur de rapports   Utilisez le mode de création de dataset partagé et enregistrez le dataset partagé sur un serveur de rapports ou un site SharePoint.  
  
-   Concepteur de rapports dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] Créez des datasets partagés sous le dossier Dataset partagé dans l’Explorateur de solutions. Pour publier un dataset partagé, déployez-le dans un serveur de rapports ou un site SharePoint.  
  
-   Charger un fichier de définition de dataset partagé (.rsd)   Vous pouvez charger un fichier sur le serveur de rapports ou le site SharePoint. Sur un site SharePoint. Un fichier téléchargé n'est pas validé par rapport au schéma jusqu'à ce que le dataset partagé soit en cache ou soit utilisé dans un rapport.  
  
 La définition de dataset partagé inclut une requête, des paramètres de dataset y compris les valeurs par défaut, des options de données telles que le respect de la casse, et des filtres de dataset. Les valeurs que vous établissez dans la définition sont utilisées chaque fois que le dataset partagé est inclus dans un rapport.  
  
 Pour utiliser un dataset partagé dans un rapport, vous ouvrez une application telle que le Générateur de rapports, accédez au serveur de rapports ou au site SharePoint, et sélectionnez le dataset partagé. Cela ajoute une instance du dataset partagé au rapport. Dans le rapport, vous ne pouvez pas afficher ou modifier la requête ou la source de données partagée pour le dataset partagé. Vous pouvez spécifier un jeu supplémentaire de valeurs de propriété du dataset qui s'appliquent à l'instance dans le rapport. Par exemple, vous pouvez ajouter un filtre ou des options de modifications des données telles que le respect de la casse. Pour plus d’informations, consultez [Datasets incorporés dans le rapport et datasets partagés &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/report-embedded-datasets-and-shared-datasets-report-builder-and-ssrs.md) dans la [documentation du Générateur de rapports ](http://go.microsoft.com/fwlink/?LinkId=154494) sur msdn.microsoft.com.  
  
## <a name="managing-shared-datasets"></a>Gestion de Datasets partagés  
 Pour gérer les propriétés d'un dataset partagé, vous pouvez utiliser le Gestionnaire de rapports pour un serveur de rapports en mode natif ou des pages d'application sur un site SharePoint si vous avez déployé le serveur de rapports en mode intégré SharePoint. Les tâches que vous pouvez effectuer sur un dataset partagé dépendent de vos attributions de rôle ainsi que du niveau sur site et des autorisations au niveau de l'élément, notamment les autorisations sur le dossier si l'héritage des autorisations est appliqué. La sécurité au niveau de l'élément pour les datasets partagés suit le même modèle que celle pour les rapports. Pour plus d’informations, consultez [Sécuriser les éléments de dataset partagés](../../reporting-services/security/secure-shared-dataset-items.md).  
  
 Vous pouvez gérer les propriétés de l'élément du dataset partagé, notamment la source de données partagée à utiliser, indépendamment du rapport qui utilise le dataset partagé ou de la source de données partagée dont il dépend. Pour modifier la requête ou d'autres propriétés du dataset qui font partie de la définition de dataset partagée, vous devez modifier la définition.  
  
### <a name="manage-shared-dataset-item-properties"></a>Gérer des propriétés d'élément de dataset partagé  
 Le tableau suivant répertorie les propriétés d'élément que vous pouvez modifier pour un élément de dataset partagé.  
  
|||  
|-|-|  
|Modifier le nom|Modifiez le nom du dataset partagé. Toutes les références d'éléments dépendants continueront de fonctionner.|  
|Modifier la description|Modifiez la description du dataset partagé.|  
|Modifier le délai d'attente d'exécution de la requête|Définissez le délai d'attente d'exécution de la requête en secondes. Zéro (0) seconde signifie aucun délai d'attente. Détermine le nombre de secondes avant l'expiration de la requête du dataset. Pour ne spécifier aucune valeur de délai d'attente, utilisez 0. Pour plus d’informations, consultez [Définition des valeurs de délai d’attente pour le traitement d’un rapport et d’un dataset partagé &#40;SSRS&#41;](../../reporting-services/report-server/setting-time-out-values-for-report-and-shared-dataset-processing-ssrs.md).|  
|Afficher les éléments dépendants|Affichez les éléments qui utilisent ce dataset partagé : parties de rapport publiées, sources de données partagées et rapports.|  
  
 Les propriétés de dataset partagées supplémentaires suivantes sont configurées automatiquement :  
  
|Propriété|Description|  
|--------------|-----------------|  
|HasDataSourceCredentials|Si la source de données partagée associée a des informations d'identification enregistrées sur le serveur de rapports.|  
|HasUserProfileDependencies|Si le rapport a une référence à la collection globale Utilisateur dans sa requête ou dans les expressions de filtre.|  
  
## <a name="viewing-or-changing-the-shared-dataset-definition"></a>Affichage ou modification de la définition du dataset partagé  
 Les propriétés de dataset partagé, notamment la requête, les paramètres de dataset, les valeurs par défaut, les filtres de dataset et les options de données telles que le classement et le respect de la casse, sont enregistrées dans la définition de dataset partagé. Si vous avez des autorisations suffisantes, vous pouvez afficher et modifier la définition.  
  
 Pour afficher ou modifier la définition de dataset partagé, modifiez le dataset partagé dans une application telle que le Générateur de rapports en mode création de dataset partagé. Après avoir apporté des modifications, enregistrez la définition de dataset partagé de nouveau sur le serveur ou le site.  
  
 Une autre méthode pour consulter la définition de dataset partagé dans XML consiste à utiliser la syntaxe d'accès URL dans le Gestionnaire de rapports. Par exemple, vous pouvez utiliser la commande de l'accès URL suivante pour afficher une définition de dataset partagé nommée DataSet1 à partir du serveur de rapports pour consulter les valeurs par défaut pour chaque paramètre de dataset :  
  
```  
http://localhost/reportserver/?/DataSet1&rs:command=GetShareddatasetDefinition  
```  
  
## <a name="controlling-access-to-the-shared-dataset-definition"></a>Contrôle de l'accès à la définition du dataset partagé  
 Par défaut, les tâches suivantes s'appliquent aux opérations sur les datasets partagés.  
  
-   **Afficher les rapports** Afficher les éléments de dataset partagés et les propriétés d’élément.  
  
-   **Lire les rapports** Lire les définitions de dataset partagé.  
  
-   **Gérer les rapports** Créer et supprimer les datasets partagés et modifier les propriétés de dataset partagé.  
  
-   **Définir la sécurité sur les éléments** Afficher et modifier les paramètres de sécurité pour les datasets partagés.  
  
 Pour plus d’informations sur quelles tâches et autorisations contrôlent l’accès aux propriétés de la source des données sur un serveur de rapports en mode natif, consultez [Sécuriser les éléments de dataset partagés](../../reporting-services/security/secure-shared-dataset-items.md).  
  
 Les autorisations d'afficher et de modifier les propriétés des éléments dans une bibliothèque SharePoint sont déterminées par l'administrateur de site. Pour plus d’informations, consultez [Article de référence sur les autorisations de site SharePoint et de listes pour les éléments de serveur de rapports](../../reporting-services/security/sharepoint-site-and-list-permission-reference-for-report-server-items.md).  
  
## <a name="how-to-work-with-shared-dataset-properties-on-a-report-server"></a>Comment utiliser les propriétés des datasets partagés sur un serveur de rapports  
 Vous pouvez utiliser divers outils pour travailler avec les datasets partagés. Le tableau suivant résume les approches et les outils, et fournit un lien vers des instructions supplémentaires.  
  
|Tâche|Outil|Lien|  
|----------|----------|----------|  
|Ajouter un dataset partagé ou modifier les propriétés de la définition du dataset partagé.|Enregistrer dans le Générateur de rapports.<br /><br /> Déployer dans le Concepteur de rapports.<br /><br /> Télécharger un fichier .rsd dans le Gestionnaire de rapports|[Datasets incorporés dans le rapport et datasets partagés &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-data/report-embedded-datasets-and-shared-datasets-report-builder-and-ssrs.md) dans la [documentation du Générateur de rapports ](http://go.microsoft.com/fwlink/?LinkId=154494) sur msdn.microsoft.com.<br /><br /> [Page Charger un fichier &#40;Gestionnaire de rapports&#41;](http://msdn.microsoft.com/library/7bb3166f-9374-4449-b66a-ffb77298507d)<br /><br /> Si vous téléchargez un dataset partagé avant que la source de données partagée dont il dépend soit publiée, vous devez lier manuellement le dataset partagé à la source de données partagée. Pour plus d’informations, consultez [Page Propriétés générales, Datasets partagés &#40;Gestionnaire de rapports&#41;](http://msdn.microsoft.com/library/10798e41-24c3-4e69-893b-7ee6af7fc958).|  
|Modifier les propriétés d'élément de dataset partagé.|Gestionnaire de rapports|[Page Propriétés générales, Datasets partagés &#40;Gestionnaire de rapports&#41;](http://msdn.microsoft.com/library/10798e41-24c3-4e69-893b-7ee6af7fc958)|  
|Spécifier des propriétés de dataset partagé supplémentaires pour une instance de dataset partagé dans un rapport.|Générateur de rapports Concepteur de rapports|[Boîte de dialogue Propriétés du dataset, Requête](http://msdn.microsoft.com/library/1fa34a4b-7de0-4e92-99fa-bc28a206773f)|  
|Créer une liaison avec une source de données partagée différente pour un dataset partagé.|Gestionnaire de rapports|[Page Sélection de la source de données &#40;Gestionnaire de rapports&#41;](http://msdn.microsoft.com/library/7f7e8b19-0c0b-4b1f-9cc1-057099aa07eb)|  
|Vérifiez les valeurs par défaut pour les paramètres de dataset.|Ouvrez dans le Générateur de rapports ou utilisez la syntaxe de l'accès URL.|Exemple :<br /><br /> `http://localhost/reportserver/?/DataSet1&rs:command=GetShareddatasetDefinition`|  
|Activer la mise en cache|Gestionnaire de rapports|[Mettre en cache les datasets partagés &#40;SSRS&#41;](../../reporting-services/report-server/cache-shared-datasets-ssrs.md)<br /><br /> [Page Mise en cache, datasets partagés &#40;Gestionnaire de rapports&#41;](http://msdn.microsoft.com/library/eac372e9-d2a1-48a8-bbe5-09d101df16ea)|  
|Créer ou modifier un plan d'actualisation du cache|Gestionnaire de rapports|[Options d’actualisation du cache &#40;Gestionnaire de rapports&#41;](http://msdn.microsoft.com/library/227da40c-6bd2-48ec-aa9c-50ce6c1ca3a6)|  
|Consulter le schéma de définition de dataset partagé.|Gestionnaire de rapports|`http://<reportserver>/shareddatasetdefinition.xsd`|  
|En mode intégré SharePoint, synchroniser la définition de dataset partagé entre le serveur de rapports et le site SharePoint|Pages d’application SharePoint|Modifier les propriétés d'élément de dataset partagé<br /><br /> Modifier les options du cache<br /><br /> Modifier la source de données partagée|  
  
## <a name="comparing-shared-datasets-with-other-report-server-items"></a>Comparaison de datasets partagés avec d'autres éléments de serveur de rapports  
 Lorsque vous gérez plusieurs types d'éléments sur un serveur de rapports, il est utile de comprendre en quoi les éléments sont similaires et en quoi ils sont différents d'autres éléments du serveur de rapports.  
  
 Les datasets partagés sont semblables aux sources de données partagées et aux rapports sur les points suivants :  
  
-   Tout comme les sources de données partagées, les datasets partagés sont gérés indépendamment des rapports dans lesquels ils sont utilisés. Une partie de la gestion d'un dataset partagé sur un serveur de rapports consiste à être capable de modifier la source de données partagée dont il dépend, sans modifier la définition du dataset partagé.  
  
-   Comme les rapports, les datasets partagés peuvent être mis en cache. Les informations d'identification requises par la source de données doivent respecter les restrictions de mise en cache et des valeurs par défaut doivent être spécifiées pour chaque paramètre. Pour plus d’informations, consultez [Mettre en cache les datasets partagés &#40;SSRS&#41;](../../reporting-services/report-server/cache-shared-datasets-ssrs.md).  
  
-   Comme pour les rapports, chaque fois que le traitement a lieu, la définition actuelle de l'élément sur le serveur de rapports est utilisée. Si vous apportez des modifications à un dataset partagé, chaque rapport qui l'utilise se servira de la définition actuelle sur le serveur de rapports lors du traitement du rapport. Si la mise en cache est activée pour le dataset partagé et que vous apportez des modifications à la définition de dataset partagé, les modifications ne sont utilisées qu'à l'expiration des données dans le cache. Vous pouvez utiliser des plans d'actualisation de cache pour aider à fournir un jeu de données cohérent pour plusieurs rapports.  
  
 Les datasets partagés se différencient des parties de rapport publiées comme suit :  
  
-   Contrairement aux parties de rapport publiées, des modifications apportées à la définition de dataset partagée sur un serveur de rapports ne déclenchent pas de notifications de mise à jour lorsque le rapport est ouvert dans un client de création de rapports. Lorsque vous exécutez le rapport, les données de la définition actuelle de dataset partagé sur le serveur de rapports sont utilisées.  
  
 Les datasets partagés sont semblables aux abonnements sur les points suivants :  
  
-   Les datasets partagés peuvent utiliser des planifications spécifiques aux éléments et partagées pour la mise en cache.  
  
-   Les datasets partagés suivent les mêmes règles pour la spécification des valeurs de paramètre que les abonnements.  
  
## <a name="see-also"></a> Voir aussi  
 [Gestion du contenu du serveur de rapports &#40;SSRS en mode natif&#41;](../../reporting-services/report-server/report-server-content-management-ssrs-native-mode.md)   
 [Octroi d'autorisations sur un serveur de rapports en mode natif](../../reporting-services/security/granting-permissions-on-a-native-mode-report-server.md)  
  
  
