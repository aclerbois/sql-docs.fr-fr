---
title: Administration de serveur de tableau croisé dynamique et de Configuration dans l’Administration centrale de l’alimentation | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: ppvt-sharepoint
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: fd681ec1859195219f73f6aacc6fd1f8665681a6
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="power-pivot-server-administration-and-configuration-in-central-administration"></a>Administration et configuration d’un serveur Power Pivot dans l’Administration centrale
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  [!INCLUDE[ssGemini_md](../../includes/ssgemini-md.md)] sont gérées par les administrateurs d’application de service SharePoint à l’aide de l’Administration centrale SharePoint.  
  
 [!INCLUDE[ssGemini_md](../../includes/ssgemini-md.md)] pour SharePoint doit être configuré avant de pouvoir être utilisé. Après avoir installé [!INCLUDE[ssGemini_md](../../includes/ssgemini-md.md)] pour SharePoint à l’aide du programme d’installation de SQL Server, vous pouvez le configurer selon l’une des approches suivantes :  
  
-   [!INCLUDE[ssGemini_md](../../includes/ssgemini-md.md)] ou outil de configuration de [!INCLUDE[ssGemini_md](../../includes/ssgemini-md.md)] pour SharePoint 2013  
  
-   Démarrez l'Administration centrale de SharePoint.  
  
-   Applets de commande PowerShell  
  
 Ces trois méthodes permettent d'obtenir un serveur entièrement configuré.  
  
 Cette section présente les tâches requises pour configurer le logiciel à l'aide de l'Administration centrale. Au minimum, vous devez effectuer les trois tâches de configuration obligatoires répertoriées dans la liste ci-dessous.  
  
> [!IMPORTANT]  
>  Pour SharePoint 2010, vous devez installer SharePoint 2010 Service Pack 1 (SP1) avant de pouvoir configurer [!INCLUDE[ssGemini_md](../../includes/ssgemini-md.md)] pour SharePoint, ou une batterie de serveurs SharePoint qui utilise un serveur de base de données [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] . Si vous n'avez pas encore installé le Service Pack, faites-le maintenant avant de commencer la configuration du serveur.  
  
## <a name="benefits-of-configuring-power-pivot-for-sharepoint-using-central-administration"></a>Avantages de la configuration de Power Pivot pour SharePoint à l’aide de l’Administration centrale  
 L'Administration centrale de SharePoint est l'application d'administration d'une batterie de serveurs SharePoint. Si vous êtes administrateur de batterie, vous préférerez peut-être utiliser un outil familier pour ajouter une instance de [!INCLUDE[ssGemini_md](../../includes/ssgemini-md.md)] pour SharePoint à votre batterie.  
  
 Contrairement aux outils de configuration de [!INCLUDE[ssGemini_md](../../includes/ssgemini-md.md)] ou aux applets de commande PowerShell, l’Administration centrale fournit des pages qui spécifient toutes les options que vous pouvez définir lors de la configuration d’une application ou d’un serveur. Les autres méthodes réduisent le nombre d'étapes impliquées dans le flux de travail de configuration, ou nécessitent des connaissances préalables concernant la configuration d'un serveur SharePoint à l'aide de PowerShell.  
  
## <a name="related-content"></a>Contenu connexe  
 [Configuration de Power Pivot à l’aide de Windows PowerShell](../../analysis-services/power-pivot-sharepoint/power-pivot-configuration-using-windows-powershell.md)  
  
 [Outils de configuration de Power Pivot](../../analysis-services/power-pivot-sharepoint/power-pivot-configuration-tools.md)  
  
## <a name="related-tasks"></a>Tâches associées  
  
|Lien|Type|Description de la tâche|  
|----------|----------|----------------------|  
|[Déployer des solutions Power Pivot sur SharePoint](../../analysis-services/power-pivot-sharepoint/deploy-power-pivot-solutions-to-sharepoint.md)|Requis|Cette étape installe les fichiers solutions qui ajoutent les fichiers programme et les pages d'application à la batterie de serveurs et aux collections de sites.|  
|[Création et configuration d’une application de service Power Pivot dans l'Administration centrale](../../analysis-services/power-pivot-sharepoint/create-and-configure-power-pivot-service-application-in-ca.md)|Requis|Cette étape configure le service système [!INCLUDE[ssGemini_md](../../includes/ssgemini-md.md)] .|  
|[Activer l’intégration des fonctionnalités Power Pivot pour des collections de sites dans l’Administration centrale](../../analysis-services/power-pivot-sharepoint/activate-power-pivot-integration-for-site-collections-in-ca.md)|Requis|Cette étape active les fonctionnalités [!INCLUDE[ssGemini_md](../../includes/ssgemini-md.md)] au niveau de la collection de sites.|  
|[Ajouter MSOLAP.5 en tant que fournisseur de données approuvé dans Excel Services](../../analysis-services/power-pivot-sharepoint/add-msolap-5-as-a-trusted-data-provider-in-excel-services.md)|Requis|Cette étape ajoute le fournisseur OLE DB Analysis Services en tant que fournisseur approuvé dans Excel Services.|  
|[Actualisation des données Power Pivot avec SharePoint 2010](http://msdn.microsoft.com/en-us/01b54e6f-66e5-485c-acaa-3f9aa53119c9)|Recommandation|L'actualisation des données est facultative mais recommandée. Elle vous permet de planifier les mises à jour sans assistance des données [!INCLUDE[ssGemini_md](../../includes/ssgemini-md.md)] dans les classeurs Excel publiés.|  
|[Configurer le compte d’actualisation des données PowerPivot sans assistance (PowerPivot pour SharePoint)](http://msdn.microsoft.com/en-us/81401eac-c619-4fad-ad3e-599e7a6f8493)|Recommandation|Cette étape configure un compte spécial qui peut être utilisé pour exécuter des travaux d'actualisation des données sur le serveur.|  
|[Configurer la collecte des données d’utilisation &#40;PowerPivot pour SharePoint](../../analysis-services/power-pivot-sharepoint/configure-usage-data-collection-for-power-pivot-for-sharepoint.md)|Ce paramètre est facultatif|La collecte des données d'utilisation est configurée par défaut. Vous pouvez utiliser ces étapes pour modifier les paramètres par défaut.|  
|[Configurer un traitement d’actualisation des données uniquement ou de requêtes uniquement (Power Pivot pour SharePoint)](http://msdn.microsoft.com/en-us/5e027605-1086-4941-bb01-f315df8f829b)|Ce paramètre est facultatif|Une instance [!INCLUDE[ssGemini_md](../../includes/ssgemini-md.md)] peut être consacrée uniquement aux travaux ou requêtes d’actualisation des données. En outre, vous pouvez modifier les paramètres par défaut pour les travaux parallèles d'actualisation des données.|  
|[Configurer des comptes de service Power Pivot](../../analysis-services/power-pivot-sharepoint/configure-power-pivot-service-accounts.md)|Ce paramètre est facultatif|Explique comment mettre à jour les mots de passe ou modifier les comptes de service.|  
|[Connecter une application de service Power Pivot à une application web SharePoint dans l’Administration centrale](../../analysis-services/power-pivot-sharepoint/connect-power-pivot-service-app-to-sharepoint-web-app-in-ca.md)|Ce paramètre est facultatif|Explique comment modifier les associations de service.|  
|[Créer un emplacement approuvé pour les sites Power Pivot dans l’Administration centrale](../../analysis-services/power-pivot-sharepoint/create-a-trusted-location-for-power-pivot-sites-in-central-administration.md)|Ce paramètre est facultatif|Explique comment ajouter la galerie [!INCLUDE[ssGemini_md](../../includes/ssgemini-md.md)] en tant qu’emplacement approuvé.|  
|[Configurer et afficher les fichiers journaux SharePoint et la journalisation des diagnostics &#40;Power Pivot pour SharePoint&#41;](../../analysis-services/power-pivot-sharepoint/configure-and-view-sharepoint-and-diagnostic-logging.md)|Ce paramètre est facultatif|La journalisation des événements est configurée par défaut. Vous pouvez utiliser ces étapes pour modifier les paramètres par défaut.|  
|[Configurer des règles d’intégrité Power Pivot](../../analysis-services/power-pivot-sharepoint/configure-power-pivot-health-rules.md)|Ce paramètre est facultatif|Les règles d'intégrité du serveur sont configurées par défaut. Vous pouvez utiliser ces étapes pour modifier certains paramètres par défaut.|  
|[Créer et personnaliser une Galerie PowerPivot](../../analysis-services/power-pivot-sharepoint/create-and-customize-power-pivot-gallery.md)|Ce paramètre est facultatif|Pour les installations que vous configurez manuellement, cette procédure explique comment créer une bibliothèque Galerie [!INCLUDE[ssGemini_md](../../includes/ssgemini-md.md)] qui affiche des images miniatures des classeurs [!INCLUDE[ssGemini_md](../../includes/ssgemini-md.md)] qu’elle contient.|  
|[Ajouter un type de contenu de connexion du modèle sémantique BI à une bibliothèque &#40;Power Pivot pour SharePoint&#41;](../../analysis-services/power-pivot-sharepoint/add-bi-semantic-model-connection-content-type-to-library.md)|Ce paramètre est facultatif|Explique comment étendre une bibliothèque de documents pour prendre en charge la création de fichiers de connexion de modèles sémantiques BI.|  
  
## <a name="see-also"></a>Voir aussi  
 [Installation de Power Pivot pour SharePoint 2010](http://msdn.microsoft.com/en-us/8d47dde7-c941-4280-a934-e2fe3f9a938f)   
 [Référence de paramètre de configuration &#40;Power Pivot pour SharePoint&#41;](../../analysis-services/power-pivot-sharepoint/configuration-setting-reference-power-pivot-for-sharepoint.md)   
 [Récupération d’urgence pour PowerPivot pour SharePoint](http://go.microsoft.com/fwlink/p/?LinkId=389570)  
  
  
