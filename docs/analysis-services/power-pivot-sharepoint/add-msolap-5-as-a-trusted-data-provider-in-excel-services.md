---
title: Ajouter MSOLAP.5 en tant qu’un fournisseur de données approuvé dans Excel Services | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: ppvt-sharepoint
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: a3ba11e1242a7deb63e19221985e1e56f83acb94
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="add-msolap5-as-a-trusted-data-provider-in-excel-services"></a>Ajouter MSOLAP.5 en tant que fournisseur de données approuvé dans Excel Services
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  MSOLAP.5 fait référence au fournisseur OLE DB Analysis Services pour SQL Server 2012. Excel Services doit approuver ce fournisseur avant d’effectuer la demande de connexion qui se traduira par la disponibilité des données [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] sur un serveur.  
  
 Si vous avez configuré [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour SharePoint à l’aide de l’outil de configuration [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] , MSOLAP.5 est peut-être déjà un fournisseur approuvé, car l’outil inclut une action qui répond à cette exigence. Toutefois, si vous utilisez PowerShell, l’administration centrale, ou si vous avez exclu l’action du fournisseur approuvé dans l’outil de configuration, le fournisseur peut être manquant, auquel cas vous devez l’ajouter maintenant, dans le cadre de la configuration de la batterie de serveurs pour l’accès aux données [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] .  
  
 Vous ne devez effectuer cette étape qu'une seule fois pour chaque application de service Excel Services.  
  
 Chaque serveur physique qui traite une requête de données [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] , tel que [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour SharePoint ou un serveur Excel Services, doit avoir installé le fournisseur OLE DB sur l’ordinateur. Une installation de [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour SharePoint inclut toujours le fournisseur OLE DB, mais si Excel Services s’exécute sur un ordinateur qui ne dispose pas de [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour SharePoint, vous devez installer le fournisseur manuellement. Pour plus d’informations, voir [Installer le fournisseur OLE DB Analysis Services sur les serveurs SharePoint](http://msdn.microsoft.com/en-us/2c62daf9-1f2d-4508-a497-af62360ee859).  
  
## <a name="add-a-trusted-provider-to-excel-services"></a>Ajouter un fournisseur approuvé à Excel Services  
  
1.  Dans l'administration centrale, cliquez sur **Gérer les applications de service**, puis cliquez sur l'application de service Excel Services.  
  
2.  Cliquez sur **Fournisseurs de données approuvés**.  
  
3.  Vérifiez que MSOLAP.5 figure dans la liste. Selon la manière dont vous avez configuré [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour SharePoint, MSOLAP.5 peut être déjà approuvé.  
  
4.  S'il n'est pas répertorié, cliquez sur **Ajouter un fournisseur de données approuvé**.  
  
5.  Pour l’ID du fournisseur, tapez **MSOLAP.5**.  
  
6.  Pour le type de fournisseur, vérifiez que OLE DB est sélectionné.  
  
7.  Dans la description du fournisseur, tapez **Fournisseur Microsoft OLE DB pour OLAP Services 11.0**.  
  
  
