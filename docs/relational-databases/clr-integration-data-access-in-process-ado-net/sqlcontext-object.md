---
title: Objet SqlContext | Documents Microsoft
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
- Windows identity [CLR integration]
- SqlContext object
- context [CLR integration]
ms.assetid: 67437853-8a55-44d9-9337-90689ebba730
caps.latest.revision: 54
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: 5c2dafdda18684151ce2846300bc4abf8f50b7bd
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlcontext-object"></a>Objet SqlContext
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Vous appelez le code managé dans le serveur lorsque vous appelez une procédure ou une fonction, lorsque vous appelez une méthode sur un type CLR défini par l'utilisateur ou lorsque votre action active un déclencheur défini dans l'un des langages du [!INCLUDE[msCoName](../../includes/msconame-md.md)] .NET Framework. Comme l'exécution de ce code est demandée dans le cadre d'une connexion utilisateur, l'accès au contexte de l'appelant à partir du code en cours d'exécution dans le serveur est requis. De plus, certaines opérations d'accès aux données ne peuvent être valides que si elles sont exécutées sous le contexte de l'appelant. Par exemple, l'accès à des pseudo-tables insérées et supprimées utilisées dans les opérations de déclencheur n'est valide que sous le contexte de l'appelant.  
  
 Le contexte de l’appelant est abstrait dans un **SqlContext** objet. Pour plus d’informations sur la **SqlTriggerContext** méthodes et propriétés, consultez la **Microsoft.SqlServer.Server.SqlTriggerContext** classe documentation de référence dans le [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] Kit de développement logiciel.  
  
 **SqlContext** fournit l’accès aux composants suivants :  
  
-   **SqlPipe**: le **SqlPipe** objet représente le « canal » via lequel les résultats transitent vers le client. Pour plus d’informations sur la **SqlPipe** d’objets, consultez [SqlPipe, objet](../../relational-databases/clr-integration-data-access-in-process-ado-net/sqlpipe-object.md).  
  
-   **SqlTriggerContext**: le **SqlTriggerContext** objet peut uniquement être récupéré à partir d’un déclencheur CLR. Il fournit des informations sur l'opération qui a provoqué l'activation du déclencheur et un mappage des colonnes mises à jour. Pour plus d’informations sur la **SqlTriggerContext** d’objets, consultez [SqlTriggerContext, objet](../../relational-databases/clr-integration-data-access-in-process-ado-net/sqltriggercontext-object.md).  
  
-   **IsAvailable**: le **IsAvailable** propriété est utilisée pour déterminer la disponibilité du contexte.  
  
-   **WindowsIdentity**: le **WindowsIdentity** propriété est utilisée pour extraire l’identité Windows de l’appelant.  
  
## <a name="determining-context-availability"></a>Détermination de la disponibilité du contexte  
 Requête le **SqlContext** classe pour voir si le code en cours d’exécution est en cours d’exécution dans le processus. Pour ce faire, vérifiez la **IsAvailable** propriété de la **SqlContext** objet. Le **IsAvailable** propriété est en lecture seule et retourne **True** si le code appelant s’exécute à l’intérieur de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et si d’autres **SqlContext** membres sont accessibles. Si le **IsAvailable** propriété renvoie **False**, toutes les autres **SqlContext** membres lèvent une **InvalidOperationException**, le cas échéant. Si **IsAvailable** retourne **False**, toute tentative d’ouvrir un objet de connexion qui a « connexion contextuelle = true » dans la chaîne de connexion échoue.  
  
## <a name="retrieving-windows-identity"></a>Extraction de l'identité Windows  
 Le code CLR qui s'exécute à l'intérieur de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est toujours appelé dans le contexte du compte de processus. Si le code doit exécuter certaines actions à l’aide de l’identité de l’utilisateur appelant, à la place de la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] traiter l’identité, puis un jeton d’emprunt d’identité doit être obtenu via la **WindowsIdentity** propriété de la **SqlContext** objet. Le **WindowsIdentity** propriété retourne un **WindowsIdentity** instance représentant le [!INCLUDE[msCoName](../../includes/msconame-md.md)] identité Windows de l’appelant, ou null si le client a été authentifié à l’aide de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’authentification. Seuls les assemblys marqués avec **EXTERNAL_ACCESS** ou **UNSAFE** autorisations peuvent accéder à cette propriété.  
  
 Après avoir obtenu le **WindowsIdentity** objet, les appelants peuvent emprunter l’identité du compte client et effectuer des actions sur eux.  
  
 L’identité de l’appelant est uniquement disponible via **SqlContext.WindowsIdentity** si le client qui a initié l’exécution de la procédure stockée ou une fonction est connecté au serveur à l’aide de l’authentification Windows. Si l'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a été utilisé à la place, cette propriété a la valeur Null et le code ne peut pas emprunter l'identité de l'appelant.  
  
### <a name="example"></a>Exemple  
 L'exemple suivant montre comment obtenir l'identité Windows du client appelant et emprunter l'identité du client.  
  
 C#  
  
```  
[Microsoft.SqlServer.Server.SqlProcedure]  
public static void WindowsIDTestProc()  
{  
    WindowsIdentity clientId = null;  
    WindowsImpersonationContext impersonatedUser = null;  
  
    // Get the client ID.  
    clientId = SqlContext.WindowsIdentity;  
  
    // This outer try block is used to thwart exception filter   
    // attacks which would prevent the inner finally   
    // block from executing and resetting the impersonation.  
    try  
    {  
        try  
        {  
            impersonatedUser = clientId.Impersonate();  
            if (impersonatedUser != null)  
            {  
                // Perform some action using impersonation.  
            }  
        }  
        finally  
        {  
            // Undo impersonation.  
            if (impersonatedUser != null)  
                impersonatedUser.Undo();  
        }  
    }  
    catch  
    {  
        throw;  
    }  
}  
```  
  
 Visual Basic  
  
```  
<Microsoft.SqlServer.Server.SqlProcedure()> _  
Public Shared Sub  WindowsIDTestProcVB ()  
    Dim clientId As WindowsIdentity  
    Dim impersonatedUser As WindowsImpersonationContext  
  
    ' Get the client ID.  
    clientId = SqlContext.WindowsIdentity  
  
    ' This outer try block is used to thwart exception filter   
    ' attacks which would prevent the inner finally   
    ' block from executing and resetting the impersonation.  
  
    Try  
        Try  
  
            impersonatedUser = clientId.Impersonate()  
  
            If impersonatedUser IsNot Nothing Then  
                ' Perform some action using impersonation.  
            End If  
  
        Finally  
            ' Undo impersonation.  
            If impersonatedUser IsNot Nothing Then  
                impersonatedUser.Undo()  
            End If  
        End Try  
  
    Catch e As Exception  
  
        Throw e  
  
    End Try  
End Sub  
```  
  
## <a name="see-also"></a>Voir aussi  
 [SqlPipe, objet](../../relational-databases/clr-integration-data-access-in-process-ado-net/sqlpipe-object.md)   
 [SqlTriggerContext, objet](../../relational-databases/clr-integration-data-access-in-process-ado-net/sqltriggercontext-object.md)   
 [Déclencheurs CLR](http://msdn.microsoft.com/library/302a4e4a-3172-42b6-9cc0-4a971ab49c1c)   
 [Extensions spécifiques in-process de SQL Server à ADO.NET](../../relational-databases/clr-integration-data-access-in-process-ado-net/sql-server-in-process-specific-extensions-to-ado-net.md)  
  
  
