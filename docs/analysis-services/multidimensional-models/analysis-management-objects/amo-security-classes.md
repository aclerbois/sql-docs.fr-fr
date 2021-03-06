---
title: "Classes de sécurité AMO | Documents Microsoft"
ms.custom: 
ms.date: 02/14/2018
ms.prod: analysis-services
ms.prod_service: analysis-services
ms.service: 
ms.component: 
ms.reviewer: 
ms.suite: pro-bi
ms.technology: 
ms.tgt_pltfrm: 
ms.topic: reference
applies_to:
- SQL Server 2016 Preview
helpviewer_keywords:
- Analysis Management Objects, security
- security [AMO]
- AMO, security
ms.assetid: e3d5012a-8121-40de-9244-1fc824228693
caps.latest.revision: 
author: Minewiskan
ms.author: owend
manager: kfile
ms.workload: Inactive
ms.openlocfilehash: 151642efa920091a95002f0dad8857986ed98861
ms.sourcegitcommit: 7519508d97f095afe3c1cd85cf09a13c9eed345f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/15/2018
---
# <a name="amo-security-classes"></a>Classes de sécurité AMO
  Cette rubrique contient les sections suivantes :  
  
-   [Objets Role et RoleMember](#RolesMembers)  
  
-   [Objets d’autorisation](#Permissions)  
  
 L'illustration suivante montre la relation qui existe entre les classes décrites dans cette rubrique.  
  
 ![Classes de sécurité dans AMO abordées dans cette rubrique](../../../analysis-services/multidimensional-models/analysis-management-objects/media/amo-securityclasses.gif "des classes de sécurité dans AMO abordées dans cette rubrique")  
  
##  <a name="RolesMembers"></a>Objets Role et RoleMember  
 Pour créer un objet <xref:Microsoft.AnalysisServices.Role>, il convient de l'ajouter à la collection de rôles de la base de données, puis de mettre à jour l'objet <xref:Microsoft.AnalysisServices.Role> sur le serveur à l'aide de la méthode Update. Un objet <xref:Microsoft.AnalysisServices.Role> doit être mis à jour avant de pouvoir être utilisé.  
  
 Pour supprimer un <xref:Microsoft.AnalysisServices.Role> de l’objet, il doit être supprimé à l’aide de la méthode Drop de le <xref:Microsoft.AnalysisServices.Role> objet. La méthode Remove de la collection de rôles ne fait que masquer le rôle dans votre application ; elle ne supprime pas le rôle du serveur. Un objet <xref:Microsoft.AnalysisServices.Role> ne peut pas être supprimé si des autorisations lui sont associées.  
  
 Pour créer un objet <xref:Microsoft.AnalysisServices.RoleMember>, il convient d'ajouter un utilisateur à la collection de membres du rôle, puis de mettre à jour l'objet <xref:Microsoft.AnalysisServices.Role> sur le serveur à l'aide de la méthode Update. Seuls les administrateurs de serveur ou les administrateurs de base de données sont autorisés à créer des rôles. Un objet <xref:Microsoft.AnalysisServices.Role> doit être mis à jour sur le serveur avant que ses membres soient autorisés à utiliser les objets pour lequels l'utilisateur s'est vu octroyer une autorisation.  
  
 Pour supprimer un objet <xref:Microsoft.AnalysisServices.RoleMember>, il convient de le supprimer de la collection à l'aide de la méthode Remove de la collection, puis de mettre à jour le rôle à l'aide de la méthode Update.  
  
 Pour plus d'informations sur les méthodes et les propriétés disponibles pour ces objets, consultez  <xref:Microsoft.AnalysisServices.Role> et <xref:Microsoft.AnalysisServices.RoleMember> dans <xref:Microsoft.AnalysisServices>.  
  
##  <a name="Permissions"></a>Objets d’autorisation  
 Pour créer un objet <xref:Microsoft.AnalysisServices.Permission>, il convient de l'ajouter à la collection d'autorisations de l'objet et de mettre à jour l'objet <xref:Microsoft.AnalysisServices.Permission> sur le serveur à l'aide de la méthode Update.  
  
 Pour supprimer un objet <xref:Microsoft.AnalysisServices.Permission>, il est nécessaire d'utiliser la méthode Drop de ce même objet. La méthode Remove de la collection d'autorisations ne fait que masquer l'autorisation dans votre application ; elle ne supprime pas l'objet <xref:Microsoft.AnalysisServices.Permission> du serveur. Un rôle ne peut pas être supprimé si des autorisations lui sont associées.  
  
 Pour plus d’informations sur les méthodes et propriétés disponibles, consultez <xref:Microsoft.AnalysisServices.Permission> dans <xref:Microsoft.AnalysisServices>.  
  
## <a name="see-also"></a>Voir aussi  
 <xref:Microsoft.AnalysisServices>   
 [Programmation d’objets de sécurité AMO](../../../analysis-services/multidimensional-models/analysis-management-objects/programming-amo-security-objects.md)   
 [Autorisations et droits d’accès &#40; Analysis Services - données multidimensionnelles &#41;](http://msdn.microsoft.com/library/59fa3573-f985-46cb-8042-7da71bd59a7b)   
 [Présentation des Classes AMO](../../../analysis-services/multidimensional-models/analysis-management-objects/amo-classes-introduction.md)   
 [Architecture logique &#40; Analysis Services - données multidimensionnelles &#41;](../../../analysis-services/multidimensional-models/olap-logical/understanding-microsoft-olap-logical-architecture.md)   
 [Les objets de base de données &#40; Analysis Services - données multidimensionnelles &#41;](../../../analysis-services/multidimensional-models/olap-logical/database-objects-analysis-services-multidimensional-data.md)  
  
  
