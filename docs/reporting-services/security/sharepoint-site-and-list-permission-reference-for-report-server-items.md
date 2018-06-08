---
title: Article de référence sur les autorisations de site SharePoint et de listes pour les éléments de serveur de rapports | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: security
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- permissions [Reporting Services], SharePoint integrated mode
- SharePoint integration [Reporting Services], permissions
- security [Reporting Services], SharePoint integrated mode
- permission sets [Reporting Services]
ms.assetid: 1fcb27bd-4c4a-43f4-bfff-e42a59c87c49
caps.latest.revision: 14
author: markingmyname
ms.author: maghan
manager: kfile
ms.openlocfilehash: 79bfc468d44f86fad3aca24637ab66b25700cb39
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sharepoint-site-and-list-permission-reference-for-report-server-items"></a>Article de référence sur les autorisations de site SharePoint et de listes pour les éléments de serveur de rapports
  Cette rubrique constitue une référence concernant les autorisations de SharePoint utilisables pour accorder un accès à des opérations sur un serveur de rapports qui s'exécute en mode intégré SharePoint. Si vous créez des niveaux d'autorisation personnalisés, cette rubrique peut vous aider à choisir les autorisations à utiliser.  
  
 SharePoint fournit trente-trois autorisations utilisables pour contrôler l'accès au contenu et aux opérations. Certaines d'entre elles s'appliquent aux documents et aux opérations qui concernent un serveur de rapports [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Vous pouvez utiliser les tables de référence ci-dessous pour connaître les autorisations qui prennent en charge des tâches données sur les rapports.  
  
 Chaque tableau commence par une liste des autorisations SharePoint et la description correspondante. Le tableau contient trois colonnes qui indiquent comment une autorisation est utilisée dans les niveaux d'autorisation prédéfinis. Les niveaux d'autorisation prédéfinis incluent les suivants :  
  
|Niveau d'autorisation|Abréviation|  
|----------------------|------------------|  
|Contrôle total|**F**|  
|Collaboration|**C**|  
|Visiteur|**V**|  
  
 Les autorisations sans incidence sur un serveur de rapports ne sont pas répertoriées. Toutes les autorisations de personnalisation sont exclues de cet article de référence. Même si vous pouvez inclure des éléments du serveur de rapports dans un site Web personnalisé, le serveur de rapports ne traite pas directement les opérations ou les demandes de personnalisation.  
  
||  
|-|  
|[!INCLUDE[applies](../../includes/applies-md.md)]<br /><br /> [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] Mode SharePoint &#124; SharePoint 2010 et SharePoint 2013.|  
  
## <a name="list-permissions"></a>Autorisations sur les listes  
 Les autorisations définies sur la bibliothèque contenant les éléments du serveur de rapports déterminent la façon dont les utilisateurs accèdent à ces éléments.  
  
|Autorisation|Description|F|C|V|Opération de serveur de rapports|  
|----------------|-----------------|-------|-------|-------|-----------------------------|  
|Gérer les listes|Créez et supprimez des listes, ajoutez ou supprimez des colonnes dans une liste et ajoutez ou supprimez des vues publiques d'une liste.|X|||Créez un dossier dans une bibliothèque SharePoint au cours d'une opération de publication depuis un outil de création. Cette autorisation est également requise pour gérer l'historique de rapport.|  
|Ajouter des éléments|Ajoutez des éléments aux listes, des documents aux bibliothèques de documents et des commentaires aux discussions Web.|X|X||Ajoutez des rapports, des modèles de rapport, des sources de données partagées et des ressources (fichiers image externes) aux bibliothèques SharePoint. Créez des sources de données partagées. Créez des modèles de rapport à partir de sources de données partagées. Démarrez le Générateur de rapports et créez un nouveau rapport ou chargez un modèle dans le Générateur de rapports.|  
|Modifier des éléments|Modifiez des éléments dans les listes, des documents dans les bibliothèques de documents, des commentaires de discussion Web dans les documents et personnalisez les pages de composant WebPart dans les bibliothèques de documents.<br /><br /> Créez des abonnements et modifiez les abonnements que vous avez créés.|X|X||Affichez les versions précédentes d'un document ainsi que les instantanés d'historique de rapport. Modifiez les propriétés d'éléments des rapports et autres documents. Définissez les options de traitement des rapports. Définissez des paramètres sur un rapport. Modifiez les propriétés de la source de données. Créez des instantanés d'historique de rapport. Ouvrez un modèle de rapport ou un rapport basé sur un modèle dans le Générateur de rapports et enregistrez vos modifications dans le fichier. Affectez des rapports consultables à l'aide de clics à des entités d'un modèle. Remplacez une définition de rapport, une source de données partagée, un modèle de rapport ou une ressource par une version plus récente (remplace le fichier tout en préservant les métadonnées). Gérez les éléments dépendants qui sont référencés dans un rapport ou un modèle. Personnalisez le composant WebPart Visionneuse de rapports d'après un rapport spécifique.<br /><br /> Créez, modifiez et supprimez les abonnements qui utilisent les extensions de remise [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] pour remettre les rapports aux emplacements cibles. Ces actions sont réservées au propriétaire et aux utilisateurs de l'abonnement disposant de l'autorisation Gérer les alertes.|  
|Supprimer les éléments|Supprimez des éléments d'une liste, des documents d'une bibliothèque de documents et des commentaires de discussion Web dans des documents.|X|X||Supprimez des rapports, des modèles de rapport, des sources de données partagées et d'autres documents d'une bibliothèque.|  
|Afficher les éléments|Affichez les éléments des listes, les documents des bibliothèques de documents et les commentaires des discussions Web.|X|X|X|Ouvrez un rapport, un modèle de rapport ou un autre document et traitez-le sur le serveur de rapports.|  
|Ouvrir les éléments|Afficher la source des documents avec les descripteurs de fichier côté serveur.|X|X|X|Affichez une liste des sources de données partagées. Téléchargez une copie du fichier source pour une définition ou un modèle de rapport. Affichez des rapports consultables à l'aide de clics utilisant un modèle de rapport comme source de données.|  
|Afficher les versions|Affichez les versions précédentes d'un élément de liste ou d'un document.|X|X|X|Affichez les versions précédentes d'un élément et les instantanés de rapport.|  
|Supprimer les versions|Supprimez les versions précédentes d'un élément de liste ou d'un document.|X|X||Supprimez des versions précédentes d'un élément et des instantanés de rapport.|  
  
> [!NOTE]  
>  Parmi les autres autorisations de liste figurent Remplacer l'extraction, Approuver les éléments et Afficher les pages des applications. Ces autorisations ne sont pas évaluées par le serveur de rapports. Le serveur de rapports ne traite pas ces opérations.  
  
## <a name="site-permissions"></a>Autorisations sur le site  
 Les autorisations sur le site déterminent l'accès aux opérations sur le serveur de rapports qui ne sont pas directement liées aux éléments stockés dans une bibliothèque donnée. Par exemple, la création ou la gestion de planifications partagées peut servir à des éléments de plusieurs bibliothèques, et la configuration du composant WebPart Visionneuse de rapport peut être utilisée sur la totalité d'un site.  
  
|Autorisation|Description|F|C|V|Opération de serveur de rapports|  
|----------------|-----------------|-------|-------|-------|-----------------------------|  
|Gérer les autorisations|Créez et modifiez les niveaux d'autorisation sur le site Web et attribuez des autorisations aux utilisateurs et aux groupes.|X|||Vous pouvez modifier les autorisations pour tous les éléments et opérations du serveur de rapports. Vous pouvez définir la sécurité des éléments d'un modèle.|  
|Gérer le site Web|Réalisez toutes les tâches d'administration du site Web et gérez le contenu.|X|||Créez, modifiez et supprimez des planifications partagées.|  
|Ajouter et personnaliser les pages|Ajoutez, modifiez ou supprimez des pages HTML ou des pages de composant WebPart, et modifiez le site web à l’aide d’un éditeur compatible [!INCLUDE[winSPServ](../../includes/winspserv-md.md)].|X|||Ajoutez ou supprimez un composant WebPart Visionneuse de rapports.|  
|Parcourir les informations utilisateur|Affichez les informations relatives aux utilisateurs du site Web.|X|X|X|Parcourez des rapports et autres éléments sur des sites, des bibliothèques et des dossiers. Publiez des rapports et autres éléments dans une bibliothèque.|  
|Énumérer les autorisations|Énumérez les autorisations sur le site Web, la liste, le dossier, le document ou l'élément de liste.|X|||Consultez les autorisations pour tous les éléments du serveur de rapports. Affichez un rapport consultable à l'aide de clics qui utilise un modèle contenant des paramètres de sécurité de l'élément de modèle.|  
|Gérer les alertes|Gérez les alertes pour tous les utilisateurs du site Web.|X|||Créez, modifiez et supprimez un abonnement quelconque d'un site.|  
|Utiliser les interfaces distantes|Utilisez les interfaces SOAP, Web DAV ou SharePoint Designer pour accéder au site Web.|X|X|X|Sert à appeler le point de terminaison du proxy URL vers le serveur de rapports.|  
|Ouvrir|Ouvrez un site Web, une liste ou un dossier pour accéder aux éléments qu'il contient.|X|X|X|Consultez les planifications et les propriétés des éléments.|  
  
## <a name="see-also"></a> Voir aussi  
 [Comparer des rôles et des tâches dans Reporting Services avec les autorisations et les groupes SharePoint](../../reporting-services/security/reporting-services-roles-tasks-vs-sharepoint-groups-permissions.md)   
 [Accord d'autorisations sur des éléments de serveur de rapports sur un site SharePoint](../../reporting-services/security/granting-permissions-on-report-server-items-on-a-sharepoint-site.md)  
  
  
