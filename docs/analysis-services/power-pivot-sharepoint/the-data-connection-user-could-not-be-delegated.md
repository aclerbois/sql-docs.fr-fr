---
title: L’utilisateur de connexion de données n’a pas pu être déléguée | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: ppvt-sharepoint
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: cbf9b41b58e4c492c4b278aa4cad60fa26dbcb08
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="the-data-connection-user-could-not-be-delegated"></a>L’utilisateur de connexion de données n’a pas pu être déléguée.
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Pour les classeurs Excel contenant des données [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] , Excel Services retourne cette erreur s’il ne peut pas se connecter à une instance de serveur [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] dans SharePoint.  
  
## <a name="details"></a>Détails  
  
|||  
|-|-|  
|S'applique à|[!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour SharePoint|  
|Version du produit|[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]|  
|Cause|Échec de connexion lors d’une tentative d’utilisation d’un fournisseur de données [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] .|  
|Texte du message|La connexion de données utilise l'authentification Windows, et les informations d'identification utilisateur n'ont pas pu être déléguées. Échec de l’actualisation des connexions suivantes : données [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)]|  
  
## <a name="explanation"></a>Explication  
 Ce message d'erreur peut s'afficher pour plusieurs raisons. Le facteur qu'ils ont en commun est qu'Excel Services ne peut pas obtenir une identité de l'utilisateur Windows valide à partir d'un jeton de revendications dans SharePoint. Dans le cas des classeurs Excel contenant des données [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] , cette erreur se produit quand l’une des conditions suivantes est respectée :  
  
-   Le service d'émission de jetons Revendications vers Windows n'est pas en cours d'exécution. Vous pouvez vérifier la cause de cette erreur en consultant le fichier journal SharePoint. Si les journaux SharePoint incluent le message « Le point de terminaison du canal 'net.pipe://localhost/s 4u/022694f3-9fbd-422b-b4b2-312e25dae2a2' est introuvable sur la machine locale », le service d'émission de jetons Revendications vers Windows n'est pas en cours d'exécution. Pour le démarrer, utilisez l'Administration centrale, puis vérifiez que le service est en cours d'exécution dans l'application console Services.  
  
-   Aucun contrôleur de domaine n'est disponible pour valider l'identité de l'utilisateur. Le service d'émission de jetons Revendications vers Windows n'utilise pas des informations d'identification mises en cache. Il valide l'identité de l'utilisateur pour chaque connexion. Vous pouvez vérifier la cause de cette erreur en consultant le fichier journal SharePoint. Si les journaux SharePoint incluent le message « Échec de l'obtention de WindowsIdentity à partir de IClaimsIdentity », l'identité de l'utilisateur n'a pas pu être authentifiée.  
  
-   Les ordinateurs doivent être membres du même domaine ou se trouver dans des domaines qui bénéficient dune relation d'approbation bidirectionnelle.  
  
-   Vous devez utiliser des comptes d'utilisateur de domaine Windows. Les comptes doivent avoir un nom d'utilisateur principal (UPN).  
  
-   Le compte de service Excel Services doit disposer d'autorisations Active Directory pour interroger l'objet.  
  
## <a name="user-action"></a>Action de l'utilisateur  
 Utilisez les instructions suivantes pour vérifier l'état du service d'émission de jetons Revendications vers Windows.  
  
 Pour tous les autres scénarios, contactez votre administrateur réseau.  
  
#### <a name="enable-claims-to-windows-token-service"></a>Activer le service d'émission de jetons Revendications vers Windows  
  
1.  Dans l'Administration centrale, sous Paramètres système, cliquez sur **Gérer les services sur le serveur**.  
  
2.  Sélectionnez **Service d'émission de jetons Revendications vers Windows**, puis cliquez sur **Démarrer**.  
  
3.  Vérifiez que le service est également en cours d'exécution dans la console Services :  
  
    1.  Dans Outils d'administration, cliquez sur Services.  
  
    2.  Démarrez le service d'émission de jetons Revendications vers Windows s'il n'est pas en cours d'exécution.  
  
## <a name="see-also"></a>Voir aussi  
 [Configuration des comptes de service Power Pivot](../../analysis-services/power-pivot-sharepoint/configure-power-pivot-service-accounts.md)  
  
  
