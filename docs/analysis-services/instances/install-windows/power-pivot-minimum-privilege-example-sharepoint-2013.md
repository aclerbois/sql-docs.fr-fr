---
title: Power Pivot avec privilèges Minimum exemple - SharePoint 2013 | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: ppvt-sharepoint
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: c58a33f7a8c1ab0e8676f6d32b14b03274707c9e
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="power-pivot-minimum-privilege-example---sharepoint-2013"></a>Power Pivot avec privilèges Minimum exemple - SharePoint 2013
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  Cette rubrique décrit un exemple de configuration de [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] pour SharePoint 2013 avec les privilèges minimum. La configuration utilise un compte différent pour chacun des trois composants et chaque compte dispose du niveau minimum de privilèges.  
  
## <a name="summary-of-accounts"></a>Récapitulatif des comptes  
 [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] pour SharePoint 2013 prend en charge l’utilisation du compte Service réseau pour le compte de service Analysis Services. Le compte Service réseau n'est pas un scénario pris en charge avec SharePoint 2010. Pour plus d’informations sur les comptes de Service, consultez [configurer les comptes de Service Windows et les autorisations](http://msdn.microsoft.com/library/ms143504.aspx) (http://msdn.microsoft.com/library/ms143504.aspx).  
  
 Le tableau suivant récapitule les trois comptes utilisés dans cet exemple de configuration avec privilèges minimums.  
  
|Portée|Nom|  
|-----------|----------|  
|Compte Administrateur SharePoint|**SPAdmin**|  
|Compte de batterie de serveurs SharePoint|**SPFarm**|  
|Compte de service Analysis Services|**SPsvc**|  
  
### <a name="the-sharepoint-administrator-account-spadmin"></a>Compte Administrateur SharePoint (SpAdmin)  
 **SPAdmin** est un compte de domaine que vous utilisez pour installer et configurer la batterie de serveurs. Il s’agit du compte utilisé pour exécuter l’Assistant Configuration SharePoint et l’outil de configuration [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] pour SharePoint 2013. Le compte **SPAdmin** est le seul compte à exiger des droits d’administrateur local. Avant d’exécuter l’outil de configuration [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] , accordez les privilèges du compte **SPAdmin** à l’instance de base de données SQL Server où SharePoint crée les bases de données de contenu et de configuration. Pour configurer le compte SPAdmin dans un scénario avec privilèges minimums, il doit être membre des rôles **securityadmin** et **dbcreator**.  
  
### <a name="the-farm-account-spfarm"></a>Compte de batterie de serveurs (SPFarm)  
 **SPFarm** est un compte de domaine que le service du minuteur SharePoint et l'application Web de l'Administration centrale utilisent pour accéder à la base de données de contenu SharePoint. Ce compte n'a pas besoin d'être administrateur local. L’Assistant Configuration SharePoint octroie le privilège minimal approprié dans la base de données SQL Server principale. La configuration de privilèges minimums SQL Server correspond à l’appartenance aux rôles **securityadmin** et **dbcreator**.  
  
### <a name="the-service-account-for-power-pivot-service-spsvc"></a>Compte de service pour le service Power Pivot (SPsvc)  
 Si une nouvelle batterie de serveurs SharePoint n’est pas configurée avant d’exécuter l’outil de configuration [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] , l’outil de configuration [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] créera par défaut les applications suivantes :  
  
-   [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] .  
  
-   Application Excel Services.  
  
-   Application Banque d'informations sécurisée.  
  
 L’outil de configuration [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] configure les trois applications de service dans le pool d’applications par défaut. Ce pool d'applications est généralement configuré pour s'exécuter sous le compte SPFarm, lequel a accès à un grand nombre de ressources dont un compte de service n'a pas besoin. Pour transformer l'environnement en un environnement de privilèges minimums, configurez un nouveau compte de domaine qui sera utilisé par le pool d'applications et l'application Web appropriés.  
  
 **Pour créer un compte de domaine SPsvc à utiliser comme compte de service SharePoint :**  
  
1.  Dans l’Administration centrale de SharePoint, sélectionnez **Sécurité**.  
  
2.  Sélectionnez **Configurer les comptes de service**.  
  
3.  Sélectionnez **Enregistrer le nouveau compte géré**.  
  
 Le compte **SPSvc** ne dispose d'aucun privilège d'administrateur local et SPsvc ne disposera d'aucun privilège dans la base de données SharePoint. SPsvc requiert uniquement les droits d’administration sur l’instance [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] d’Analysis Services.  
  
 **Pour configurer le pool d'applications approprié pour utiliser le compte de SPsvc :**  
  
1.  Dans l’Administration centrale de SharePoint, sélectionnez **Sécurité**.  
  
2.  Sélectionnez **Configurer les comptes de service**.  
  
3.  Sélectionnez le pool d’application de service utilisé par l’application de service [!INCLUDE[ssGemini](../../../includes/ssgemini-md.md)] . Ensuite, sélectionnez le compte SPSvc.  
  
 **Pour accorder l'accès à l'application Web avec PowerShell :**  
  
1.  Exécutez SharePoint 2013 Management Shell avec des privilèges d'administrateur.  
  
2.  Exécutez le code PowerShell suivant :  
  
    ```  
    $webApp = Get-SPWebApplication "http://<servername>"  
    $webApp.GrantAccessToProcessIdentity("DOMAIN\<ServiceAccountName>")  
  
    ```  
  
  
