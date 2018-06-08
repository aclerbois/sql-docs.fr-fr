---
title: Renommer une Instance Analysis Services | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: ''
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 6214dbcef4036bc545a931f90ee8dca4580ef287
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="rename-an-analysis-services-instance"></a>Renommer une instance d'Analysis Services
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Vous pouvez renommer une instance existante de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] à l’aide de l’utilitaire **Modification du nom d’instance** , installé avec Management Studio (installation web).  
  
> [!IMPORTANT]  
>  Pendant que l’instance est renommée, l’utilitaire Modification du nom d’instance [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] s’exécute avec des privilèges élevés et met à jour le nom de service Windows, les comptes de sécurité et les entrées de Registre associées à cette instance. Pour vous assurer que ces actions sont effectuées, veillez à exécuter cet outil en tant qu'administrateur système local.  
  
 L’utilitaire Modification du nom d’instance [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] ne modifie pas le dossier du programme créé pour l’instance d’origine. Ne modifiez pas le nom du dossier du programme pour qu'il corresponde à l'instance que vous renommez. Le fait de modifier un nom de dossier de programme peut empêcher au programme d'installation de réparer ou désinstaller celle-ci.  
  
> [!NOTE]  
>  L’utilitaire Modification du nom d’instance de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] n’est pas pris en charge pour une utilisation dans un environnement de cluster.  
  
### <a name="to-rename-an-instance-of-analysis-services"></a>Pour renommer une instance d'Analysis Services  
  
1.  Lancez l’outil **Modification du nom d’instance** , **asinstancerename.exe**, à partir de C:\Program Files (x86)\Microsoft SQL Server\130\Tools\Binn\ManagementStudio.  
  
2.  Dans la boîte de dialogue **Renommer l’instance** , sélectionnez dans la liste **Instance à renommer** l’instance que vous souhaitez renommer.  
  
3.  Dans la zone **Nouveau nom d’instance** , entrez le nouveau nom de l’instance.  
  
4.  Vérifiez que le nom d’utilisateur et le mot de passe sont corrects, puis cliquez sur **Renommer**.  
  
     L'instance Analysis Services sera arrêtée et redémarrée dans le cadre du changement de nom.  
  
### <a name="post-rename-checklist"></a>Liste de contrôle après la modification du nom  
  
1.  Pour récupérer l'accès aux bases de données qui s'exécutent sur l'instance renommée, vous devez mettre à jour manuellement les connexions de données dans Excel ou les autres applications clientes. Vérifiez également toutes les connexions prédéfinies, telles que les sources de données partagées Reporting Services, les fichiers ODC Excel ou les fichiers de connexion de modèle sémantique BI qui peuvent référencer l'instance que vous venez de renommer. Pour plus d’informations, consultez [Se connecter à Analysis Services](../../analysis-services/instances/connect-to-analysis-services.md).  
  
2.  Mettez à jour les scripts PowerShell ou AMO (Analysis Management Objects) que vous utilisez régulièrement pour la sauvegarde, la synchronisation ou le traitement des bases de données.  
  
3.  Mettez à jour les propriétés des projets [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] dans lesquels vous travaillez dans [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)]. Pour les instances de serveur en mode tabulaire, veillez à mettre à jour la propriété Serveur d'espace de travail dans le fichier model.bim, ainsi que la propriété Serveur dans le projet.  
  
4.  Selon la façon dont vous avez spécifié le compte de service, vous devrez peut-être mettre à jour les connexions à la base de données ou les autorisations de fichiers qui accordent des droits d'accès au service (par exemple, si vous utilisez le compte de service pour le traitement des données ou pour accéder aux objets liés sur un autre serveur).  
  
     La mise à jour d'une connexion à une base de données ou des autorisations de fichiers est nécessaire si vous avez utilisé un compte virtuel pour configurer le service. Les comptes virtuels sont basés sur le nom de l'instance, donc si vous renommez l'instance, le compte virtuel est également mis à jour. Cela signifie que toutes les connexions ou autorisations précédentes créées pour l'instance antérieure ne sont plus valides.  
  
     L'exemple suivant en est l'illustration. Supposons que vous avez installé un serveur en mode tabulaire en tant qu'instance nommée « tabular » à l'aide du compte virtuel par défaut, ce qui entraîne la configuration suivante :  
  
    1.  Nom de l’instance = \<serveur > \TABULAR  
  
    2.  Nom du service = MSOLAP$TABULAR  
  
    3.  Compte virtuel = NT Service\ MSOLAP$TABULAR  
  
     Supposons à présent que vous renommez l'instance en « TAB2 ». Suite au changement de nom, la configuration se présente à présent comme suit :  
  
    1.  Nom de l’instance = \<serveur > \TAB2  
  
    2.  Nom du service = MSOLAP$TAB2  
  
    3.  Compte virtuel = NT Service\ MSOLAP$TAB2  
  
     Comme vous pouvez le voir, les autorisations d'accès à la base de données et aux fichiers précédemment accordées au service « NT Service\ MSOLAP$TABULAR » ne sont plus valides. Pour vous assurer que les tâches et les opérations effectuées par le service s'exécutent comme précédemment, vous devez maintenant accorder au service « NT Service\ MSOLAP$TAB2 » les autorisations d'accès à la nouvelle base de données et aux fichiers.  
  
  
