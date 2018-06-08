---
title: Impossible d’actualiser les données pour une connexion de données dans le classeur | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: ppvt-sharepoint
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: d1fabd45d3b9858114e48e3bdde258ed6ccc8362
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="unable-to-refresh-data-for-a-data-connection-in-the-workbook"></a>Impossible d'actualiser les données pour une connexion de données dans le classeur
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Pour les classeurs Excel qui contiennent des données [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] , Excel Services retourne cette erreur en cas de demande de connexion à un serveur [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] qui échoue.  
  
## <a name="details"></a>Détails  
  
|||  
|-|-|  
|S'applique à :|[!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour SharePoint|  
|Version du produit|[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]|  
|Cause|Voir ci-dessous.|  
|Texte du message|Impossible d'actualiser les données pour une connexion de données dans le classeur. Essayez encore ou contactez votre administrateur système. Les connexions suivantes n’ont pas pu s’actualiser : données de [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)]|  
  
## <a name="explanation-and-resolution"></a>Explication et résolution  
 Excel Services ne peut pas se connecter aux données [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] ou les charger. Les conditions qui provoquent cette erreur sont les suivantes :  
  
 **Scénario 1 : le service n'est pas démarré**  
  
 L’instance de SQL Server Analysis Services ([!INCLUDE[ssGemini](../../includes/ssgemini-md.md)]) n’est pas démarrée. Un mot de passe périmé arrête l'exécution du service. Pour plus d'informations sur la modification du mot de passe, consultez [Configuration des comptes de service Power Pivot](../../analysis-services/power-pivot-sharepoint/configure-power-pivot-service-accounts.md) et [Démarrer ou arrêter un serveur PowerPivot pour SharePoint](../../analysis-services/power-pivot-sharepoint/start-or-stop-a-power-pivot-for-sharepoint-server.md).  
  
 **Scénario 2a : Ouverture d'un classeur de version antérieure sur le serveur**  
  
 Le classeur que vous essayez d’ouvrir peut avoir été créé dans la version SQL Server 2008 R2 de [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour Excel. Très probablement, le fournisseur de données Analysis Services qui est spécifié dans la chaîne de connexion de données n'est pas présent sur l'ordinateur qui gère la demande.  
  
 Si c’est le cas, vous trouverez ce message dans le journal ULS : « Échec de l’actualisation '[!INCLUDE[ssGemini](../../includes/ssgemini-md.md)]t données » dans le classeur '\<URL du classeur >' », suivi par « Impossible d’obtenir une connexion ».  
  
 Pour déterminer la version du classeur, ouvrez-le dans Excel et vérifiez quel fournisseur de données est spécifié dans la chaîne de connexion. Un classeur SQL Server 2008 R2 utilise MSOLAP.4 comme fournisseur de données.  
  
 Pour contourner ce problème, vous pouvez mettre à niveau le classeur. Ou bien, vous pouvez installer les bibliothèques clientes de la version SQL Server 2008 R2 d’Analysis Services sur des ordinateurs physiques exécutant [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour SharePoint ou Excel Services. Voir [Installer le fournisseur OLE DB Analysis Services sur les serveurs SharePoint](http://msdn.microsoft.com/en-us/2c62daf9-1f2d-4508-a497-af62360ee859).  
  
 **Scénario 2b : Excel Services s'exécute sur un serveur d'applications qui a une version incorrecte des bibliothèques clientes**  
  
 Par défaut, SharePoint Server 2010 installe la version SQL Server 2008 du fournisseur OLE DB Analysis Services sur les serveurs d'applications qui exécutent Excel Services. Dans une batterie de serveurs qui prend en charge l’accès aux données [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] , tous les serveurs physiques exécutant des applications qui demandent des données [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] , par exemple Excel Services et [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour SharePoint, doivent utiliser une version ultérieure du fournisseur de données.  
  
 Les serveurs qui exécutent [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour SharePoint obtiennent le fournisseur de données OLE DB mis à jour automatiquement. D’autres serveurs, tels que ceux qui exécutent une instance autonome d’Excel Services sans [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour SharePoint sur le même ordinateur, doivent être corrigés pour utiliser les bibliothèques clientes plus récentes. Pour plus d’informations, voir [Installer le fournisseur OLE DB Analysis Services sur les serveurs SharePoint](http://msdn.microsoft.com/en-us/2c62daf9-1f2d-4508-a497-af62360ee859).  
  
 **Scénario 3 : le contrôleur de domaine n'est pas disponible**  
  
 La cause de l'erreur peut être qu'un contrôleur de domaine n'est pas disponible pour valider l'identité de l'utilisateur. Un contrôleur de domaine est requis par le service d'émission de jetons Revendications vers Windows pour authentifier l'utilisateur SharePoint à chaque connexion. Le service d'émission de jetons Revendications vers Windows n'utilise pas des informations d'identification mises en cache. Il valide l'identité de l'utilisateur pour chaque connexion.  
  
 Vous pouvez vérifier la cause de cette erreur en consultant le fichier journal SharePoint. Si les journaux SharePoint incluent le message « Échec de l'obtention de WindowsIdentity à partir de IClaimsIdentity », l'identité de l'utilisateur n'a pas pu être authentifiée.  
  
 Pour contourner ce problème, joignez l’ordinateur au même domaine que le serveur [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] ou installez un contrôleur de domaine sur votre ordinateur local. La deuxième solution, l'installation du contrôleur de domaine, oblige à créer des comptes de domaine locaux pour tous les services et utilisateurs. Vous devrez configurer des comptes de service et des autorisations SharePoint pour les comptes que vous définissez.  
  
 L’installation d’un contrôleur de domaine sur votre ordinateur est utile si votre objectif est d’utiliser [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour SharePoint hors connexion. Pour obtenir des instructions détaillées sur l’utilisation de [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] hors connexion, consultez le billet de blog pour « prendre votre [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] serveur sur le réseau » sur [ http://www.powerpivotgeek.com ](http://go.microsoft.com/fwlink/?LinkId=184241).  
  
 **Scénario 4 : serveur instable**  
  
 Un ou plusieurs services peuvent être dans un état incohérent. Dans certains cas, l'exécution d'IISRESET résoudra le problème.  
  
  
