---
title: Récupération d’urgence et disponibilité PowerPivot de l’alimentation | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: ppvt-sharepoint
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: b2b5a16ac487b52f3592743481e0013b4bf44856
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="power-pivot-availability-and-disaster-recovery"></a>Récupération d’urgence et disponibilité PowerPivot
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  La disponibilité et les plans de récupération d'urgence pour [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] dépendent principalement de la conception de votre batterie de serveurs SharePoint, de la durée de temps mort acceptable pour les différents composants, ainsi que des outils et des meilleures pratiques que vous implémentez pour la disponibilité SharePoint. Cette rubrique présente une synthèse des différentes technologies et inclut des exemples de schémas de topologie à prendre en compte lorsque vous planifiez la disponibilité et la récupération d'urgence pour un déploiement [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] .  
  
||  
|-|  
|**[!INCLUDE[applies](../../includes/applies-md.md)]**  SharePoint 2013 &#124; SharePoint 2010|  
  
 **Dans cette rubrique :**  
  
-   [Exemple de topologie SharePoint 2013 pour une haute disponibilité de PowerPivot](#bkmk_sharepoint2013)  
  
-   [Exemple de topologie SharePoint 2010 pour une haute disponibilité de PowerPivot](#bkmk_sharepoint2010)  
  
-   [Base de données d'application de service PowerPivot et disponibilité SQL Server et technologies de récupération](#bkmk_sql_server_technologies)  
  
-   [Liens vers des informations supplémentaires](#bkmk_more_resources)  
  
##  <a name="bkmk_sharepoint2013"></a> Exemple de topologie SharePoint 2013 pour une haute disponibilité de PowerPivot  
 Dans un déploiement [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] 2013, il existe davantage de souplesse dans la façon dont vous concevez la disponibilité de [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] . Dans SharePoint 2013, l'instance [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] déployée en mode SharePoint, également appelée serveur [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] , s'exécute en dehors de la batterie de serveurs SharePoint et peut être installée sur des serveurs distincts. Chaque instance d' [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] en mode SharePoint est inscrite avec Excel Services. Le service partagé [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] et l'application de service s'exécutent sur des serveurs d'applications SharePoint.  
  
 Le schéma suivant illustre un exemple de déploiement [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] 2013. Cet exemple prend en charge une bonne disponibilité des services [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] et suppose une sauvegarde régulière des bases de données.  
  
 ![disponibilité PowerPivot dans 2013](../../analysis-services/power-pivot-sharepoint/media/ssas-powerpivot-services-2013.png "disponibilité powerpivot dans 2013")  
  
-   **(1)** Serveurs web frontaux. Utilisez le complément [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] 2013 pour installer les fournisseurs de données sur chaque serveur. Pour plus d’informations, consultez [Installer ou désinstaller le complément Power Pivot pour SharePoint &#40;SharePoint 2013&#41;](../../analysis-services/instances/install-windows/install-or-uninstall-the-power-pivot-for-sharepoint-add-in-sharepoint-2013.md).  
  
-   **(2)** Le service partagé [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] s’exécute **sur chaque** serveur d’applications et permet à l’application de service de s’exécuter **sur** différents serveurs d’applications. Par conséquent, si un serveur d'applications est mis hors connexion, l'application [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] reste toujours disponible.  
  
-   **(3)** Les services de calcul Excel s’exécutent sur chaque serveur d’applications et permettent à l’application de service de s’exécuter sur différents serveurs d’applications. Par conséquent, si un serveur d'applications est mis hors connexion, Excel Calculation Services reste toujours disponible.  
  
-   **(4)** et **(6)** Les instances de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] en mode SharePoint s’exécutent sur des serveurs en dehors de la batterie de serveurs SharePoint ; cela inclut le service Windows **SQL Server Analysis Services ([!INCLUDE[ssGemini](../../includes/ssgemini-md.md)])**. Chacune de ces instances est inscrite auprès d’Excel Services **(3)**. Excel Services gère l'équilibrage de charge des demandes effectuées aux serveurs [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] . L'architecture de [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] 2013 vous permet d'avoir plusieurs serveurs pour [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] . Vous pouvez ainsi ajouter facilement des instances, si nécessaire. Pour plus d’informations, consultez [Gérer les paramètres de modèle de données Excel Services (SharePoint Server 2013)](http://technet.microsoft.com/library/jj219780\(v=office.15\).aspx).  
  
-   **(5)** Bases de données SQL Server utilisées pour le contenu, la configuration et les bases de données d’application. Cela inclut la base de données d'application de service [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] . Le plan de récupération d'urgence doit inclure la couche de base de données. Dans cette conception, les bases de données s’exécutent sur le même serveur que **(4)** l’une des instances [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] . **(4)** et **(5)** peuvent également se trouver sur des serveurs différents.  
  
-   **(7)** Forme de sauvegarde ou de redondance de base de données SQL Server.  
  
##  <a name="bkmk_sharepoint2010"></a> Exemple de topologie SharePoint 2010 pour une haute disponibilité de PowerPivot  
 L'architecture de [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] 2010 requiert que tous les composants de [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] s'exécutent sur les mêmes serveurs d'applications SharePoint. Cela inclut l'instance de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] déployée en mode SharePoint et deux services partagés, par rapport à un seul dans un déploiement SharePoint 2013.  
  
 Le schéma suivant illustre un exemple de déploiement [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] 2010. Cet exemple prend en charge une bonne disponibilité des services [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] et suppose une sauvegarde régulière des bases de données.  
  
 ![disponibilité PowerPivot dans sharepoint 2010](../../analysis-services/power-pivot-sharepoint/media/ssas-powerpivot-services-2010.png "disponibilité powerpivot dans sharepoint 2010")  
  
-   **(1)** Serveurs web frontaux. Installez les fournisseurs de données sur chaque serveur. Pour plus d’informations, voir [Installer le fournisseur OLE DB Analysis Services sur les serveurs SharePoint](http://msdn.microsoft.com/en-us/2c62daf9-1f2d-4508-a497-af62360ee859).  
  
-   **(2)** Les deux services partagés [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] et **(4)** le service Windows **SQL Server Analysis Services ([!INCLUDE[ssGemini](../../includes/ssgemini-md.md)])** sont installés sur les serveurs d’applications SharePoint.  
  
     Le service système [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] s'exécute **sur chaque** serveur d'applications et permet à l'application de service de s'exécuter **sur** différents serveurs d'applications. Si un serveur d'applications est mis hors connexion, l'application de service [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] reste toujours disponible.  
  
     Pour augmenter la capacité de [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] dans SharePoint 2010, déployez davantage de serveurs d'applications SharePoint qui exécutent le service système [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] .  
  
-   **(3)** Les services de calcul Excel s’exécutent sur chaque serveur d’applications et permettent à l’application de service de s’exécuter sur différents serveurs d’applications. Par conséquent, si un serveur d'applications est mis hors connexion, Excel Calculation Services reste toujours disponible.  
  
-   **(5)** Bases de données SQL Server utilisées pour le contenu, la configuration et les bases de données d’application. Cela inclut la base de données d'application de service [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] . Le plan de récupération d'urgence doit inclure la couche de base de données.  
  
-   **(6)** Forme de sauvegarde ou de redondance de base de données SQL Server.  
  
##  <a name="bkmk_sql_server_technologies"></a> Base de données d'application de service PowerPivot et disponibilité SQL Server et technologies de récupération  
 Incluez la base de données d’application de service [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] dans votre planification pour une disponibilité élevée SharePoint. Le nom par défaut de la base de données est `DefaultPowerPivotServiceApplicationDB-<GUID>`. Voici un résumé des technologies de disponibilité [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et des recommandations pour une utilisation avec la base de données [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] . Pour plus d’informations, consultez [Options de haute disponibilité et de récupération d’urgence prises en charge pour les bases de données SharePoint (SharePoint 2013)](http://technet.microsoft.com/library/jj841106.aspx).  
  
||Commentaires|  
|-|--------------|  
|[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] et [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] dans une batterie de serveurs pour la disponibilité.|Prise en charge mais non recommandée. La recommandation consiste à utiliser AlwaysOn en mode de validation synchrone.|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] en mode de validation synchrone|Pris en charge et recommandé.|  
|[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] mise en miroir asynchrone ou envoi de journaux à une autre batterie de serveurs pour la récupération d’urgence.|Pris en charge.|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] avec validation asynchrone pour la récupération d’urgence|Pris en charge|  
  
-   [!INCLUDE[ssHADR](../../includes/sshadr-md.md)]  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Copie des journaux de transactions  
  
 Pour plus d'informations sur la façon de planifier un scénario de reprise progressive avec [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)], consultez [Récupération d'urgence pour PowerPivot](http://social.technet.microsoft.com/wiki/contents/articles/22137.sharepoint-powerpivot-disaster-recovery.aspx).  
  
## <a name="verification"></a>Vérification  
 Pour obtenir des directives et des scripts permettant de vérifier un déploiement [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] avant et après un cycle de récupération d’urgence, consultez [Liste de vérification : utiliser PowerShell pour vérifier Power Pivot pour SharePoint](../../analysis-services/instances/install-windows/checklist-use-powershell-to-verify-power-pivot-for-sharepoint.md).  
  
##  <a name="bkmk_more_resources"></a> Liens vers des informations supplémentaires  
  
-   [Options de haute disponibilité et de récupération d’urgence prises en charge pour les bases de données SharePoint (SharePoint 2013)](http://technet.microsoft.com/library/jj841106.aspx)  
  
-   [Plan de récupération d'urgence (SharePoint Server 2010)](http://technet.microsoft.com/library/ff628971\(v=office.14\).aspx)  
  
-   [Livre blanc sur la sauvegarde et la récupération de SQL Server Cloud](http://www.microsoft.com/server-cloud/solutions/cloud-backup-recovery.aspx?WT.srch=1&WT.mc_ID=SEM_BING_USEvergreenSearch_Unassigned&CR_CC=Unassigned#fbid=RjU2Nbzu2dT)  
  
-   [Outil Microsoft® SQL Server Backup to Microsoft Windows® Azure®](http://www.microsoft.com/download/details.aspx?id=40740)  
  
 **Contenu de la communauté**  
  
-   [Gestion des instances de service dans SharePoint 2013](http://www.petri.co.il/manage-service-instances-sharepoint-2013.htm)  
  
