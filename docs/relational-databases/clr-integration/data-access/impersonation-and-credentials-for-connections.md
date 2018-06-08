---
title: L’emprunt d’identité et les informations d’identification pour les connexions | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: clr
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- impersonation [CLR integration]
- security [CLR integration]
- database objects [CLR integration], connections
- connections [CLR integration]
- authentication [CLR integration]
- user impersonation [CLR integration]
- credentials [CLR integration]
- database objects [CLR integration], security
ms.assetid: 293dce7d-1db2-4657-992f-8c583d6e9ebb
caps.latest.revision: 31
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: 09e9a65b9a6be2e1dc9de9153054f9b024818d0f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="impersonation-and-credentials-for-connections"></a>Emprunt d'identité et informations d'identification pour les connexions
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Dans l'intégration du CLR (Common Language Runtime) [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], l'authentification Windows est complexe à utiliser, mais elle offre une protection supérieure à l'authentification SQL Server. Lorsque vous utilisez l'authentification Windows, gardez à l'esprit les points suivants.  
  
 Par défaut, un processus SQL Server qui se connecte à Windows acquiert le contexte de sécurité du compte de service Windows SQL Server. Mais il est possible de mapper une fonction CLR à une identité de proxy afin que ses connexions sortantes aient un contexte de sécurité différent de celui du compte de service Windows.  
  
 Dans certains cas, vous souhaiterez emprunter l’identité de l’appelant à l’aide de la **SqlContext.WindowsIdentity** propriété au lieu de l’exécution en tant que le compte de service. Le **WindowsIdentity** instance représente l’identité du client qui a appelé le code appelant et est disponible uniquement lorsque le client utilise l’authentification Windows. Après avoir obtenu le **WindowsIdentity** instance, vous pouvez appeler **Impersonate** pour modifier le jeton de sécurité du thread, puis ouvrez les connexions ADO.NET part du client.  
  
 Après avoir appelé SQLContext.WindowsIdentity.Impersonate, vous ne pouvez pas accéder aux données locales, et vous ne pouvez pas accéder aux données système. Pour accéder aux données à nouveau, vous devez appeler la méthode WindowsImpersonationContext.Undo.  
  
 L’exemple suivant montre comment emprunter l’identité de l’appelant à l’aide de la **SqlContext.WindowsIdentity** propriété.  
  
 Visual C#   
  
```  
WindowsIdentity clientId = null;  
WindowsImpersonationContext impersonatedUser = null;  
  
clientId = SqlContext.WindowsIdentity;  
  
// This outer try block is used to protect from   
// exception filter attacks which would prevent  
// the inner finally block from executing and   
// resetting the impersonation.  
try  
{  
   try  
   {  
      impersonatedUser = clientId.Impersonate();  
      if (impersonatedUser != null)  
         return GetFileDetails(directoryPath);  
         else return null;  
   }  
   finally  
   {  
      if (impersonatedUser != null)  
         impersonatedUser.Undo();  
   }  
}  
catch  
{  
   throw;  
}  
```  
  
> [!NOTE]  
>  Pour plus d’informations sur les modifications du comportement d’emprunt d’identité, consultez [modifications avec rupture des fonctionnalités du moteur de base de données dans SQL Server 2016](../../../database-engine/breaking-changes-to-database-engine-features-in-sql-server-2016.md).  
  
 Par ailleurs, si vous avez obtenu l'instance de l'identité [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Windows, vous ne pouvez pas, par défaut, propager cette instance à un autre ordinateur, cette opération étant restreinte par défaut par l'infrastructure de sécurité Windows. Il existe cependant un mécanisme, appelé « délégation », qui permet la propagation d'identités Windows sur plusieurs ordinateurs approuvés. Plus d’informations sur la délégation dans l’article TechNet, «[Kerberos Protocol Transition and Constrained Delegation](http://go.microsoft.com/fwlink/?LinkId=50419)».  
  
## <a name="see-also"></a>Voir aussi  
 [Objet SqlContext](../../../relational-databases/clr-integration-data-access-in-process-ado-net/sqlcontext-object.md)  
  
  
