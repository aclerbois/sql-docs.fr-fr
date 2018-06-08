---
title: Classes de sécurité AMO | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: amo
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 8ca8bf233f706883b29c518517a28216e144765d
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
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
 [Autorisations et droits d’accès & #40 ; Analysis Services - données multidimensionnelles & #41 ;](http://msdn.microsoft.com/library/59fa3573-f985-46cb-8042-7da71bd59a7b)   
 [Présentation des Classes AMO](../../../analysis-services/multidimensional-models/analysis-management-objects/amo-classes-introduction.md)   
 [Architecture logique & #40 ; Analysis Services - données multidimensionnelles & #41 ;](../../../analysis-services/multidimensional-models/olap-logical/understanding-microsoft-olap-logical-architecture.md)   
 [Les objets de base de données & #40 ; Analysis Services - données multidimensionnelles & #41 ;](../../../analysis-services/multidimensional-models/olap-logical/database-objects-analysis-services-multidimensional-data.md)  
  
  
