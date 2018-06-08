---
title: Configuration de tableau croisé dynamique à l’aide de Windows PowerShell de l’alimentation | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: ppvt-sharepoint
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 7deb70905efe8874e3b512c10eff5c0951ab4b24
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="power-pivot-configuration-using-windows-powershell"></a>Configuration de Power Pivot à l’aide de Windows PowerShell
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] inclut des applets de commande Windows PowerShell que vous pouvez utiliser pour configurer une installation de [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)]. La configuration complète d’une installation avec PowerShell requiert l’utilisation des applets de commande SharePoint et de [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour SharePoint. La majorité des configurations peut être effectuée avec un des outils de [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] . Pour plus d'informations sur les outils, consultez [Power Pivot Configuration Tools](../../analysis-services/power-pivot-sharepoint/power-pivot-configuration-tools.md).  
  
> [!IMPORTANT]  
>  Dans le cas d’une batterie de serveurs SharePoint 2010, vous devez avoir installé SharePoint 2010 SP1 pour être en mesure de configurer [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour SharePoint, ou une batterie de serveurs SharePoint qui utilise un serveur de base de données [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] . Si vous n'avez pas encore installé le Service Pack, installez-le avant de commencer la configuration du serveur.  
  
## <a name="benefits-of-configuring-power-pivot-for-sharepoint-using-powershell"></a>Avantages de la configuration de Power Pivot pour SharePoint à l’aide de PowerShell  
 Générez des fichiers de script Windows PowerShell (.ps1) pour automatiser les tâches de configuration. Cette approche est recommandée si vous avez besoin d'une installation à base de script et d'étapes de configuration que vous pouvez exécuter sur n'importe quel serveur. Vous pouvez avoir besoin d'un tel script dans le cadre d'un plan de récupération d'urgence afin de reconstruire un serveur en cas de défaillance matérielle.  
  
## <a name="view-a-list-of-the-power-pivot-cmdlets-on-a-server"></a>Afficher une liste des applets de commande Power Pivot sur un serveur  
 Pour afficher le contenu et des exemples des applets de commande [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] , consultez [Référence PowerShell pour Power Pivot pour SharePoint](../../analysis-services/powershell/powershell-reference-for-power-pivot-for-sharepoint.md).  
  
 Pour visualiser la liste des applets de commande [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] en utilisant PowerShell :  
  
1.  Ouvrez SharePoint Management Shell à l'aide de l'option **Exécuter en tant qu'administrateur** .  
  
2.  Entrez la commande suivante :  
  
    ```  
    Get-help *powerpivot*  
    ```  
  
     Vous devez voir apparaître une liste d’applets de commande dont le nom inclut [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] . Par exemple, `Get-PowerPivotServiceApplication`. Le nombre d'applets de commande disponibles dépend de la version d'Analysis Services que vous utilisez.  
  
    -   10 applets de commande avec le serveur SQL Server 2012 SP1 Analysis Services configuré en mode SharePoint et SharePoint 2013. La version 2012 SP1 utilise une nouvelle architecture qui permet au serveur d'analyse de s'exécuter en dehors de la batterie de serveurs SharePoint et nécessite moins d'applets de commande de gestion Windows PowerShell.  
  
    -   17 applets de commande avec le serveur SQL Server 2012 Analysis Services configuré en mode SharePoint et SharePoint 2010.  
  
     Si aucune commande n’est renvoyée dans la liste ou que vous obtenez un message d’erreur semblable à «`get-help could not find *powerpivot* in a help file in this session.`», consultez la section suivante de cette rubrique pour savoir comment activer les applets de commande [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] sur le serveur.  
  
     Toutes les applets de commande intègrent un système d'aide en ligne. L’exemple suivant montre comment afficher l’aide en ligne pour l’applet de commande **New-PowerPivotServiceApplication** :  
  
    ```  
    Get-help new-powerpivotserviceapplication -full  
    ```  
  
     Sinon, pour afficher uniquement les exemples, utilisez la syntaxe suivante :  
  
    ```  
    Get-help new-powerpivotserviceapplication -example  
    ```  
  
## <a name="enable-power-pivot-cmdlets-on-a-server"></a>Activer les applets de commande Power Pivot sur un serveur  
 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] sont disponibles une fois que vous avez installé [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour SharePoint et déployé la solution de batterie de serveurs. Les solutions sont déployées lorsque vous exécutez l'outil de configuration [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] . Suivez ces étapes pour pouvoir utiliser des applets de commande :  
  
1.  Ouvrez SharePoint Management Shell à l'aide de l'option **Exécuter en tant qu'administrateur** .  
  
2.  Exécutez la première applet de commande :  
  
    ```  
    Add-SPSolution –LiteralPath “C:\Program Files\Microsoft SQL Server\110\Tools\PowerPivotTools\ConfigurationTool\Resources\PowerPivotFarm.wsp”  
    ```  
  
     L'applet de commande retourne le nom de la solution, son ID, et Deployed=False. À l'étape suivante, vous déployez la solution.  
  
3.  Exécutez la deuxième applet de commande pour déployer la solution :  
  
    ```  
    Install-SPSolution –Identity PowerPivotFarm.wsp –GACDeployment -Force  
    ```  
  
4.  Fermez la fenêtre. Rouvrez-la, à nouveau à l'aide de l'option **Exécuter en tant qu'administrateur** .  
  
## <a name="related-content"></a>Contenu connexe  
 [Administration et configuration d’un serveur Power Pivot dans l’Administration centrale](../../analysis-services/power-pivot-sharepoint/power-pivot-server-administration-and-configuration-in-central-administration.md)  
  
 [Power Pivot Configuration Tools](../../analysis-services/power-pivot-sharepoint/power-pivot-configuration-tools.md)  
  
 [Référence PowerShell pour Power Pivot pour SharePoint](../../analysis-services/powershell/powershell-reference-for-power-pivot-for-sharepoint.md)  
  
  
