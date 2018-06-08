---
title: Se connecter à une base de données de modèle tabulaire | Documents Microsoft
ms.date: 05/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tabular-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: ce70f8ac3e22e31f0420762019c94fc629951a4b
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="connect-to-a-tabular-model-database"></a>Se connecter à une base de données de modèle tabulaire  
[!INCLUDE[ssas-appliesto-sqlas-aas](../../includes/ssas-appliesto-sqlas-aas.md)]
  Après avoir généré un modèle tabulaire et l'avoir déployé sur un serveur Analysis Services en mode tabulaire, vous devez définir les autorisations qui le mettent à disposition des applications clientes. Cet article explique comment les autorisations et comment se connecter à une base de données à partir d’applications clientes.  
  
> [!NOTE]  
>  Par défaut, les connexions distantes à Analysis Services ne sont pas disponibles avant d'avoir configuré le pare-feu. Assurez-vous que vous avez ouvert le port approprié si vous configurez une instance par défaut ou nommée pour les connexions clientes. Pour plus d’informations, consultez [Configurer le pare-feu Windows pour autoriser l’accès à Analysis Services](../../analysis-services/instances/configure-the-windows-firewall-to-allow-analysis-services-access.md).  
  
##  <a name="bkmk_userpermissions"></a> Autorisations de l'utilisateur sur la base de données  
 Les utilisateurs qui se connectent à des bases de données tabulaires doivent appartenir à un rôle de base de données qui spécifie l'accès en lecture.  
  
 Les rôles, et parfois l'appartenance au rôle, sont définis lorsqu'un modèle est créé dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], ou pour les modèles déployés, à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Pour plus d’informations sur la création de rôles à l’aide du Gestionnaire de rôles de [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)], consultez [créer et gérer les rôles](../../analysis-services/tabular-models/create-and-manage-roles-ssas-tabular.md). Pour plus d’informations sur la création et la gestion des rôles pour un modèle déployé, consultez [des rôles de modèle tabulaire](../../analysis-services/tabular-models/tabular-model-roles-ssas-tabular.md).  
  
> [!CAUTION]  
>  Le redéploiement d'un projet de modèle tabulaire avec les rôles définis à l'aide du Gestionnaire de rôles de [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] remplace les rôles définis dans un modèle tabulaire déployé.  
  
##  <a name="bkmk_admin"></a> Autorisations administratives sur le serveur  
 Pour les organisations qui utilisent SharePoint pour l'hébergement de classeurs Excel ou de rapports Reporting Services, une configuration supplémentaire est obligatoire pour mettre des données de modèle tabulaire à disposition des utilisateurs SharePoint. Si vous n'utilisez pas SharePoint, ignorez cette section.  
  
 La consultation de classeurs Excel ou de rapports [!INCLUDE[ssCrescent](../../includes/sscrescent-md.md)] qui contiennent des données tabulaires requiert que le compte utilisé pour exécuter Excel Services ou Reporting Services dispose des autorisations d'administrateur sur l'instance d'Analysis Services. Les autorisations administratives sont obligatoires pour que ces services soient approuvés par l'instance d'Analysis Services.  
  
#### <a name="grant-administrative-access-on-the-server"></a>Accorder l'accès d'administration sur le serveur  
  
1.  Dans l'Administration centrale, ouvrez la page Configurer les comptes de service.  
  
2.  Sélectionnez le pool d'applications de service utilisé par Excel Services. Il peut s'agir du **Pool d'applications de service. SharePoint Web Services Système** ou d'un pool d'applications personnalisé. Le compte géré utilisé par Excel Services apparaîtra dans la page.  
  
     Pour les batteries de serveurs SharePoint qui incluent Reporting Services en mode SharePoint, obtenez également les informations de compte de l'application de service Reporting Services.  
  
     Dans les étapes suivantes, vous ajouterez ces comptes au rôle serveur sur l'instance d'Analysis Services.  
  
3.  Dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], connectez -vous à l’instance d’ [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], cliquez avec le bouton droit sur l’instance de serveur, puis sélectionnez **Propriétés**. Dans l’Explorateur d’objets, cliquez avec le bouton droit sur **Rôles** , puis sélectionnez **Nouveau rôle**.  
  
4.  Dans la page Propriétés Analysis Services, cliquez sur **Sécurité**.  
  
5.  Cliquez sur **Ajouter**, puis entrez le compte utilisé par Excel Services, suivi du compte utilisé par Reporting Services.  
  
##  <a name="bkmk_excelconn"></a> Connexion à partir d'Excel ou de SharePoint  
 Les bibliothèques clientes qui fournissent l'accès aux bases de données Analysis Services peuvent être utilisées pour se connecter aux bases de données model qui s'exécutent sur un serveur en mode tabulaire. Les bibliothèques incluent le fournisseur OLE DB Analysis Services, ADOMD.NET et AMO.  
  
 Excel utilise le fournisseur OLE DB. Si vous avez installé MSOLAP.4 à partir de SQL Server 2008 R2 (nom de fichier msolap100.dll, version 10.50.1600.1), ou MSOLAP.5 (nom de fichier msolap110.dll) avec la version [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] de [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour Excel, vous disposez d’une version qui se connectera aux bases de données tabulaires.  
  
 Choisissez l'une des approches suivantes pour vous connecter aux bases de données model depuis Excel :  
  
-   Créez une connexion de données depuis Excel, à l'aide des instructions fournies dans la section suivante.  
  
-   Créez un fichier de connexion de modèle sémantique BI (.bism) dans SharePoint pour assurer la redirection vers une base de données qui s'exécute sur un serveur Analysis Services en mode tabulaire. Un fichier de connexion de modèle sémantique BI fournit une commande de bouton droit qui lance Excel à l'aide de la base de données model que vous avez spécifiée dans la connexion. Elle lance également [!INCLUDE[ssCrescent](../../includes/sscrescent-md.md)] si Reporting Services est installé. Pour plus d’informations sur la création et l’utilisation de fichiers de connexion de modèle sémantique BI, consultez [Créer une connexion de modèle sémantique BI à une base de données model tabulaire](../../analysis-services/power-pivot-sharepoint/create-a-bi-semantic-model-connection-to-a-tabular-model-database.md).  
  
-   Créez une source de données partagée Reporting Services qui référence une base de données tabulaire en tant que source de données. Vous pouvez créer la source de données partagée dans SharePoint et l'utiliser pour lancer [!INCLUDE[ssCrescent](../../includes/sscrescent-md.md)].  
  
#### <a name="connect-from-excel"></a>Se connecter depuis Excel  
  
1.  Dans Excel, sous l'onglet **Données** , dans **Obtenir des données externes**, cliquez sur **À partir d'autres sources**.  
  
2.  Sélectionnez **À partir d'Analysis Services**.  
  
3.  Pour **Nom du serveur**, spécifiez le nom de l'instance Analysis Service qui héberge la base de données. Le nom du serveur est souvent le nom de l'ordinateur qui exécute le logiciel du serveur. Si le serveur a été installé comme instance nommée, vous devez spécifier le nom au format suivant : \<nom_serveur >\\< nom_instance\>.  
  
     L'instance de serveur doit être configurée pour un déploiement tabulaire autonome et doit avoir une règle de trafic entrant qui en autorise l'accès. Pour plus d’informations, consultez [Déterminer le mode serveur d’une instance Analysis Services](../../analysis-services/instances/determine-the-server-mode-of-an-analysis-services-instance.md) et [Configurer le pare-feu Windows pour autoriser l’accès à Analysis Services](../../analysis-services/instances/configure-the-windows-firewall-to-allow-analysis-services-access.md).  
  
4.  Pour les informations d'identification, choisissez **Utiliser l'authentification Windows** si vous disposez d'autorisations de lecture sur la base de données. Sinon, choisissez **Utiliser le nom d'utilisateur et le mot de passe suivants**, puis entrez le nom d'utilisateur et le mot de passe d'un compte Windows disposant des autorisations de base de données. Cliquez sur **Suivant**.  
  
5.  Sélectionnez la base de données. Si la sélection est valide, un cube représentant un **Modèle** s'affiche pour la base de données. Cliquez sur **Suivant** , puis sur **Terminer**.  
  
 Une fois la connexion établie, vous pouvez utiliser les données pour créer un tableau croisé dynamique ou un graphique croisé dynamique. Pour plus d’informations, consultez [analyser dans Excel](../../analysis-services/tabular-models/analyze-in-excel-ssas-tabular.md).  
  
##  <a name="bkmk_sharepoint"></a> Se connecter depuis SharePoint  
 Si vous utilisez [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour SharePoint, vous pouvez créer un fichier de connexion de modèle sémantique BI dans SharePoint pour assurer la redirection vers une base de données qui s’exécute sur un serveur Analysis Services en mode tabulaire. Une connexion de modèle sémantique BI fournit un point de terminaison HTTP à une base de données. Elle simplifie également l'accès au modèle tabulaire pour les travailleurs du savoir qui utilisent régulièrement des documents sur un site SharePoint. Les travailleurs du savoir doivent simplement connaître l'emplacement du fichier de connexion de modèle sémantique BI ou son URL pour accéder aux bases de données model tabulaires. Les détails relatifs à l'emplacement du serveur ou au nom de la base de données sont inclus dans la connexion de modèle sémantique BI. Pour plus d’informations sur la création et l’utilisation de fichiers de connexion de modèle sémantique BI, consultez [Connexion de modèle sémantique BI Power Pivot &#40;.bism&#41;](../../analysis-services/power-pivot-sharepoint/power-pivot-bi-semantic-model-connection-bism.md) et [Créer une connexion de modèle sémantique BI à une base de données model tabulaire](../../analysis-services/power-pivot-sharepoint/create-a-bi-semantic-model-connection-to-a-tabular-model-database.md).  
  
##  <a name="bkmk_Tshoot"></a> Résolution des problèmes de connexion  
 Cette section décrit les causes et les étapes de résolution des problèmes qui peuvent se produire lors d'une connexion à une base de données model tabulaire.  
  
 **L'Assistant Connexion de données n'arrive pas à obtenir la liste des bases de données de la source de données spécifiée.**  
  
 Lorsque vous importez des données, cette erreur Microsoft Excel se produit lorsque vous essayez d’utiliser l’Assistant pour se connecter à une base de données de modèle tabulaire sur un serveur Analysis Services à distance, et vous ne disposez pas des autorisations suffisantes. Pour résoudre cette erreur, vous devez avoir des droits d'accès utilisateur sur la base de données. Reportez-vous aux instructions fournies plus haut dans cette rubrique pour accorder à un utilisateur l'accès aux données.  
  
 **Une erreur s'est produite lors de la tentative de connexion à la source de données externe. Impossible d’actualiser les connexions suivantes : \<nom du modèle > bac à sable**  
  
 Sur SharePoint, cette erreur Microsoft Excel se produit lorsque vous tentez une interaction de données, comme un filtrage des données, dans un tableau croisé dynamique qui utilise des données de modèle. L'erreur se produit parce que vous n'avez pas d'autorisations suffisantes sur le serveur Analysis Services distant. Pour résoudre cette erreur, vous devez avoir des droits d'accès utilisateur sur la base de données. Reportez-vous aux instructions fournies plus haut dans cette rubrique pour accorder à un utilisateur l'accès aux données.  
  
 **Une erreur s'est produite lors de cette opération. Recharger le classeur, puis que vous essayez d’effectuer cette opération.**  
  
 Sur SharePoint, cette erreur Microsoft Excel se produit lorsque vous tentez une interaction de données, comme un filtrage des données, dans un tableau croisé dynamique qui utilise des données de modèle. L'erreur se produit parce qu'Excel Services n'est pas approuvé par l'instance Analysis Services sur laquelle les données de modèle sont déployées. Pour résoudre cette erreur, accordez l'autorisation administrative Excel Services sur l'instance Analysis Services. Reportez-vous aux instructions fournies plus haut dans cette rubrique pour accorder à un administrateur les autorisations. Si l'erreur persiste, relancez le pool d'applications Excel Services.  
  
 **Une erreur s'est produite lors de la tentative de connexion à la source de données externe utilisée dans le classeur.**  
  
 Sur SharePoint, cette erreur Microsoft Excel se produit lorsque vous tentez une interaction de données, comme un filtrage des données, dans un tableau croisé dynamique qui utilise des données de modèle. L'erreur se produit parce que l'utilisateur n'a pas d'autorisations SharePoint suffisantes sur le classeur. L'utilisateur doit avoir des autorisations **Lire** ou supérieures. Les autorisations**Affichage seul** ne sont pas suffisantes pour l’accès aux données.  
  
## <a name="see-also"></a>Voir aussi  
 [Déploiement d’une solution de modèle tabulaire](../../analysis-services/tabular-models/tabular-model-solution-deployment-ssas-tabular.md)  
  
  
