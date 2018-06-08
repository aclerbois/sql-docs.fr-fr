---
title: Déployer des Solutions Power Pivot pour SharePoint | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: ppvt-sharepoint
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 51dffaf4569cf1aa0527ee0ba4d59379d4faab46
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="deploy-power-pivot-solutions-to-sharepoint"></a>Déployer des solutions Power Pivot sur SharePoint
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Utilisez les instructions suivantes pour déployer manuellement deux packages de solution qui ajoutent des fonctionnalités [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] à un environnement SharePoint Server 2010. Le déploiement des solutions est une étape indispensable pour configurer [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour SharePoint sur un serveur SharePoint 2010. Pour obtenir la liste complète des étapes nécessaires, consultez [Administration et configuration d’un serveur Power Pivot dans l’Administration centrale](../../analysis-services/power-pivot-sharepoint/power-pivot-server-administration-and-configuration-in-central-administration.md).  
  
 Pour déployer les solutions, vous pouvez également utiliser l’outil de configuration de [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] . L'utilisation de l'outil de configuration est plus simple et plus efficace pour une installation sur un serveur unique, mais vous souhaiterez peut-être recourir à l'Administration centrale et à PowerShell si vous préférez travailler avec un outil qui vous est familier ou si vous configurez plusieurs fonctionnalités en même temps. Pour plus d’informations sur l’utilisation de l’outil de configuration, consultez [Outils de configuration de Power Pivot](../../analysis-services/power-pivot-sharepoint/power-pivot-configuration-tools.md).  
  
 Avant de déployer ces solutions, vous devez d’abord installer [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour SharePoint à l’aide du support d’installation de SQL Server 2012. Le programme d'installation de SQL Server installe les packages de solution que vous êtes sur le point de déployer.  
  
 Cette rubrique contient les sections suivantes :  
  
 [Condition préalable : vérifier que l'application Web utilise l'authentification en mode classique](#bkmk_classic)  
  
 [Étape 1 : déployer la solution de batterie de serveurs](#bkmk_farm)  
  
 [Étape 2 : déployer la solution d’application Web Power Pivot sur l’Administration centrale](#deployCA)  
  
 [Étape 3 : déployer la solution d’application Web Power Pivot sur d’autres applications Web](#deployUI)  
  
 [Redéployer ou vous retirer la solution](#retract)  
  
 [À propos des solutions Power Pivot](#intro)  
  
##  <a name="bkmk_classic"></a> Condition préalable : vérifier que l'application Web utilise l'authentification en mode classique  
 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour SharePoint n’est pris en charge que pour les applications web qui utilisent l’authentification Windows en mode classique. Pour vérifier si l’application utilise le mode classique, exécutez l’applet de commande PowerShell suivante à partir de la **SharePoint 2010 Management Shell**, en remplaçant **http://\<nom de site de niveau supérieur >** avec le nom de votre site SharePoint :  
  
```  
Get-spwebapplication http://<top-level site name> | format-list UseClaimsAuthentication  
```  
  
 La valeur de retour devrait être **False**. Si la valeur est **True**, vous n’avez pas accès aux données [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] avec cette application Web.  
  
##  <a name="bkmk_farm"></a> Étape 1 : déployer la solution de batterie de serveurs  
 Cette section montre comment déployer des solutions à l’aide de PowerShell, mais vous pouvez également utiliser l’outil de configuration [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour effectuer cette tâche. Pour plus d’informations, consultez [Configurer ou réparer Power Pivot pour SharePoint 2010 (outil de configuration de Power Pivot)](http://msdn.microsoft.com/en-us/d61f49c5-efaa-4455-98f2-8c293fa50046).  
  
 Cette tâche ne doit être effectuée qu’une seule fois, après l’installation de [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour SharePoint.  
  
1.  Sur un serveur sur lequel est installé [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour SharePoint, ouvrez SharePoint 2010 Management Shell à l’aide de l’option **Exécuter en tant qu’administrateur** .  
  
2.  Exécutez l'applet de commande suivante pour ajouter la solution de batterie de serveurs.  
  
    ```  
    Add-SPSolution –LiteralPath “C:\Program Files\Microsoft SQL Server\110\Tools\PowerPivotTools\ConfigurationTool\Resources\PowerPivotFarm.wsp”  
    ```  
  
     L'applet de commande retourne le nom de la solution, son ID, et Deployed=False. À l'étape suivante, vous déploierez la solution.  
  
3.  Exécutez l'applet de commande suivante pour déployer la solution de batterie de serveurs :  
  
    ```  
    Install-SPSolution –Identity PowerPivotFarm.wsp –GACDeployment -Force  
    ```  
  
##  <a name="deployCA"></a> Étape 2 : déployer la solution d’application Web Power Pivot sur l’Administration centrale  
 Après avoir déployé la solution de batterie de serveurs, vous devez déployer la solution d'application Web sur l'Administration centrale. Cette étape ajoute le tableau de bord de gestion [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] dans l’Administration centrale.  
  
1.  Ouvrez SharePoint 2010 Management Shell à l'aide de l'option **Exécuter en tant qu'administrateur** .  
  
2.  Exécutez l'applet de commande suivante pour créer une référence à l'Administration centrale :  
  
    ```  
    $centralAdmin = $(Get-SPWebApplication -IncludeCentralAdministration | Where { $_.IsAdministrationWebApplication -eq $TRUE})  
    ```  
  
3.  Exécutez l'applet de commande suivante pour ajouter la solution de batterie de serveurs.  
  
    ```  
    Add-SPSolution –LiteralPath “C:\Program Files\Microsoft SQL Server\110\Tools\PowerPivotTools\ConfigurationTool\Resources\PowerPivotWebApp.wsp”  
    ```  
  
     L'applet de commande retourne le nom de la solution, son ID, et Deployed=False. À l'étape suivante, vous déploierez la solution.  
  
4.  Exécutez l'applet de commande suivante pour installer la solution d'application Web sur l'Administration centrale.  
  
    ```  
    Install-SPSolution -Identity PowerPivotWebApp.wsp -GACDeployment -Force -WebApplication $centralAdmin  
    ```  
  
 Maintenant que la solution d'application Web est déployée sur l'Administration centrale, vous pouvez recourir à cette dernière pour effectuer toutes les étapes de configuration restantes.  
  
##  <a name="deployUI"></a> Étape 3 : déployer la solution d’application Web Power Pivot sur d’autres applications Web  
 Dans la tâche précédente, vous avez déployé Powerpivotwebapp.wsp sur l'Administration centrale. Dans cette section, vous allez déployer powerpivotwebapp.wsp sur chaque application Web existante qui prend en charge l’accès aux données [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] . Si vous ajoutez des applications Web supplémentaires par la suite, assurez-vous que vous répétez cette étape pour chacune de ces applications.  
  
1.  Dans l'Administration centrale, sous Paramètres système, cliquez sur **Gérer les solutions de la batterie**.  
  
2.  Cliquez sur **powerpivotwebapp.wsp**.  
  
3.  Cliquez sur **Déployer la solution**.  
  
4.  Sous **Destination du déploiement**, sélectionnez l’application Web SharePoint pour laquelle vous voulez ajouter une prise en charge des fonctionnalités [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] .  
  
5.  Cliquez sur **OK**.  
  
6.  Répétez ces opérations pour les autres applications Web SharePoint qui prendront également en charge l’accès aux données [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] .  
  
##  <a name="retract"></a> Redéployer ou vous retirer la solution  
 Bien que l'Administration centrale de SharePoint permette de retirer une solution, vous n'avez pas besoin de retirer le fichier powerpivotwebapp.wsp, sauf si vous dépannez une installation de manière systématique ou si vous corrigez un problème de déploiement.  
  
1.  Dans l'Administration centrale de SharePoint 2010, sous Paramètres système, cliquez sur **Gérer les solutions de la batterie**.  
  
2.  Cliquez sur **Powerpivotwebapp.wsp**.  
  
3.  Cliquez sur **Retirer la solution**.  
  
 Si vous rencontrez des problèmes de déploiement de serveur que vous attribuez à la solution de batterie de serveurs, vous pouvez recommencer en exécutant l’option **Réparer** dans l’outil de configuration de [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] . Il est préférable de réparer les opérations via l'outil, car vous aurez moins d'étapes à effectuer. Pour plus d’informations, consultez [Configurer ou réparer Power Pivot pour SharePoint 2010 (outil de configuration de Power Pivot)](http://msdn.microsoft.com/en-us/d61f49c5-efaa-4455-98f2-8c293fa50046).  
  
 Si vous souhaitez néanmoins redéployer toutes les solutions, veillez à le faire dans cet ordre :  
  
1.  Retirez la solution d’application Web [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] de toutes les applications Web SharePoint qui l’utilisent.  
  
2.  Retirez la solution de batterie de serveurs [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] .  
  
3.  Redéployez la solution de batterie de serveurs [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] .  
  
4.  Redéployez la solution d’application Web [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] sur toutes les applications Web SharePoint.  
  
##  <a name="intro"></a> À propos des solutions Power Pivot  
 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour SharePoint utilise deux packages de solution pour déployer ses fichiers programme et ses pages d’application dans la batterie de serveurs et dans des applications Web individuelles.  
  
-   La solution de batterie est globale. Elle est déployée une fois, puis reste automatiquement à la disposition des nouveaux serveurs [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour SharePoint que vous ajouterez ultérieurement à la batterie.  
  
-   La solution d'application Web est spécifique aux applications et doit être déployée sur chaque application Web de la batterie de serveurs, notamment sur l'application Web Administration centrale.  
  
 Chaque solution est déployée différemment.  La solution de batterie de serveurs est déployée une fois, après l’installation de la première instance de [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour SharePoint. Pour déployer la solution de batterie de serveurs, utilisez l’outil de configuration de [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] ou les applets de commande PowerShell.  
  
 La solution d’application Web est d’abord déployée sur l’Administration centrale, puis sur d’autres applications Web qui prendront en charge les demandes de données [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] . Pour déployer la solution d’application Web sur l’Administration centrale, vous devez utiliser l’outil de configuration de [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] ou l’applet de commande PowerShell. Pour toutes les autres applications Web, vous pouvez déployer la solution d'application Web manuellement à l'aide de l'Administration centrale ou de PowerShell.  
  
|Solution| Description|  
|--------------|-----------------|  
|Powerpivotfarm.wsp|Ajoute Microsoft.AnalysisServices.SharePoint.Integration.dll à l'assembly global.<br /><br /> Ajoute Microsoft.AnalysisServices.ChannelTransport.dll à l'assembly global.<br /><br /> Installe des fonctionnalités et des fichiers de ressources, et enregistre des types de contenu.<br /><br /> Ajoute des modèles de bibliothèque pour la Galerie [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] et les bibliothèques de flux de données.<br /><br /> Ajoute des pages d’application pour la configuration de l’application de service, le tableau de bord de gestion [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] , l’actualisation des données et la Galerie [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] .|  
|powerpivotwebapp.wsp|Ajoute les fichiers de ressources Microsoft.AnalysisServices.SharePoint.Integration.dll au dossier des extensions du serveur Web sur le Web frontal.<br /><br /> Ajoute le service web [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] au serveur web frontal.<br /><br /> Ajoute la génération de miniatures pour la Galerie [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] .|  
  
## <a name="see-also"></a>Voir aussi  
 [Mettre à niveau Power Pivot pour SharePoint](../../database-engine/install-windows/upgrade-power-pivot-for-sharepoint.md)   
 [Administration et configuration d’un serveur Power Pivot dans l’Administration centrale](../../analysis-services/power-pivot-sharepoint/power-pivot-server-administration-and-configuration-in-central-administration.md)   
 [Configuration de Power Pivot à l’aide de Windows PowerShell](../../analysis-services/power-pivot-sharepoint/power-pivot-configuration-using-windows-powershell.md)  
  
  
