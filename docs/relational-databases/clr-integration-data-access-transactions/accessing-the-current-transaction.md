---
title: L’accès à la Transaction actuelle | Documents Microsoft
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
- current transaction access
- Current property
- Transaction class
ms.assetid: 1a4e2ce5-f627-4c81-8960-6a9968cefda2
caps.latest.revision: 17
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: b157a9c72533d6c92f71ddc93e7c8023eab6edc1
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="accessing-the-current-transaction"></a>Accès à la transaction actuelle
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Si une transaction est active au point le code du common language runtime (CLR) en cours d’exécution [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est entré, la transaction est exposée via le **transaction System.Transactions.Transaction** classe. La propriété **Transaction.Current** est utilisée pour accéder à la transaction actuelle. Dans la plupart des cas il n'est pas nécessaire d'accéder explicitement à la transaction. Pour les connexions de base de données, ADO.NET vérifie **Transaction.Current** automatiquement lorsque la méthode **Connection.Open** est appelée et inscrit de façon transparente la connexion dans cette transaction (à moins que le mot clé **Enlist** n'ait la valeur « false » dans la chaîne de connexion).  
  
 Vous souhaiter peut-être utiliser l'objet **Transaction** directement dans les scénarios suivants :  
  
-   Si vous souhaitez inscrire une ressource qui n'effectue pas d'inscription automatique ou qui, pour une raison quelconque, n'a pas été inscrite pendant l'initialisation.  
  
-   Si vous souhaitez inscrire explicitement une ressource dans la transaction.  
  
-   Si vous souhaitez mettre fin à la transaction externe à partir de votre procédure stockée ou fonction. Dans ce cas, vous utilisez <xref:System.Transactions.TransactionScope>. Par exemple, le code suivant restaure la transaction actuelle :  
  
    ```  
    using(TransactionScope transactionScope = new TransactionScope(TransactionScopeOptions.Required)) { }  
    ```  
  
 Le reste de cette rubrique décrit d'autres manières d'annuler une transaction externe.  
  
## <a name="canceling-an-external-transaction"></a>Annulation d'une transaction externe  
 Vous pouvez annuler des transactions externes à partir d'une procédure ou fonction managée des manières suivantes :  
  
-   La procédure ou fonction managée peut renvoyer une valeur en utilisant un paramètre de sortie. L’appel [!INCLUDE[tsql](../../includes/tsql-md.md)] procédure peut vérifier la valeur renvoyée et, le cas échéant, exécuter **ROLLBACK TRANSACTION**.  
  
-   La procédure ou fonction managée peut lever une exception personnalisée. L’appel [!INCLUDE[tsql](../../includes/tsql-md.md)] procédure peut intercepter l’exception levée par la procédure ou fonction managée dans un bloc try/catch et exécuter **ROLLBACK TRANSACTION**.  
  
-   La procédure ou fonction managée peut annuler la transaction actuelle en appelant la méthode **Transaction.Rollback** si une certaine condition est remplie.  
  
 Lorsqu'elle est appelée dans une procédure ou fonction managée, la méthode **Transaction.Rollback** lève une exception avec un message d'erreur ambigu et peut être encapsulée dans un bloc try/catch. Le message d'erreur ressemble au suivant :  
  
```  
Msg 3994, Level 16, State 1, Procedure uspRollbackFromProc, Line 0  
Transaction is not allowed to roll back inside a user defined routine, trigger or aggregate because the transaction is not started in that CLR level. Change application logic to enforce strict transaction nesting.  
```  
  
 Cette exception est attendue et le bloc try/catch est nécessaire pour que l'exécution du code continue. Sans le bloc try/catch, l'exception est levée immédiatement pour la procédure [!INCLUDE[tsql](../../includes/tsql-md.md)] appelante et l'exécution du code managé se termine. Lorsque l'exécution du code managé se termine, une autre exception est levée :  
  
```  
Msg 3991, Level 16, State 1, Procedure uspRollbackFromProc, Line 1   
The context transaction which was active before entering user defined routine, trigger or aggregate " uspRollbackFromProc " has been ended inside of it, which is not allowed. Change application logic to enforce strict transaction nesting. The statement has been terminated.  
```  
  
 Cette exception est également attendue et, pour que l'exécution continue, vous devez avoir un bloc try/catch autour de l'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] qui effectue l'action qui active le déclencheur. Malgré les deux exceptions levées, la transaction est restaurée et les modifications ne sont pas validées.  
  
### <a name="example"></a>Exemple  
 Voici un exemple de transaction restaurée à partir d'une procédure managée à l'aide de la méthode **Transaction.Rollback** . Remarquez le bloc try/catch autour de la méthode **Transaction.Rollback** dans le code managé. Le script [!INCLUDE[tsql](../../includes/tsql-md.md)] crée un assembly et une procédure stockée managée. Sachez que l'instruction **EXEC uspRollbackFromProc** est englobée dans un bloc try/catch afin que l'exception levée lorsque la procédure managée se termine soit interceptée.  
  
```csharp  
using System;  
using System.Data;  
using System.Data.SqlClient;  
using System.Data.SqlTypes;  
using Microsoft.SqlServer.Server;  
using System.Transactions;  
  
public partial class StoredProcedures  
{  
[Microsoft.SqlServer.Server.SqlProcedure]  
public static void uspRollbackFromProc()  
{  
   using (SqlConnection connection = new SqlConnection(@"context connection=true"))  
   {  
      // Open the connection.  
      connection.Open();  
  
      bool successCondition = true;  
  
      // Success condition is met.  
      if (successCondition)  
      {  
         SqlContext.Pipe.Send("Success condition met in procedure.");   
         // Perform other actions here.  
      }  
  
      //  Success condition is not met, the transaction will be rolled back.  
      else  
      {  
         SqlContext.Pipe.Send("Success condition not met in managed procedure. Transaction rolling back...");  
         try  
         {  
               // Get the current transaction and roll it back.  
               Transaction trans = Transaction.Current;  
               trans.Rollback();  
         }  
         catch (SqlException ex)  
         {  
            // Catch the expected exception.   
            // This allows the connection to close correctly.                      
         }    
      }  
  
      // Close the connection.  
      connection.Close();  
   }  
}  
};  
```  
  
```vb  
Imports System  
Imports System.Data  
Imports System.Data.SqlClient  
Imports System.Data.SqlTypes  
Imports Microsoft.SqlServer.Server  
Imports System.Transactions  
  
Partial Public Class StoredProcedures  
<Microsoft.SqlServer.Server.SqlProcedure()> _  
Public Shared Sub  uspRollbackFromProc ()  
   Using connection As New SqlConnection("context connection=true")  
  
   ' Open the connection.  
   connection.Open()  
  
   Dim successCondition As Boolean  
   successCondition = False  
  
   ' Success condition is met.  
   If successCondition Then  
  
      SqlContext.Pipe.Send("Success condition met in procedure.")  
  
      ' Success condition is not met, the transaction will be rolled back.  
  
   Else  
      SqlContext.Pipe.Send("Success condition not met in managed procedure. Transaction rolling back...")  
      Try  
         ' Get the current transaction and roll it back.  
         Dim trans As Transaction  
         trans = Transaction.Current  
         trans.Rollback()  
  
      Catch ex As SqlException  
         ' Catch the exception instead of throwing it.    
         ' This allows the connection to close correctly.                      
      End Try  
  
   End If  
  
   ' Close the connection.  
   connection.Close()  
  
End Using  
End Sub  
End Class  
```  
  
 **Transact-SQL**  
  
```  
--Register assembly.  
CREATE ASSEMBLY TestProcs FROM 'C:\Programming\TestProcs.dll'   
Go  
CREATE PROCEDURE uspRollbackFromProc AS EXTERNAL NAME TestProcs.StoredProcedures.uspRollbackFromProc  
Go  
  
-- Execute procedure.  
BEGIN TRY  
BEGIN TRANSACTION   
-- Perform other actions.  
Exec uspRollbackFromProc  
-- Perform other actions.  
PRINT N'Commiting transaction...'  
COMMIT TRANSACTION  
END TRY  
  
BEGIN CATCH  
SELECT ERROR_NUMBER() AS ErrorNum, ERROR_MESSAGE() AS ErrorMessage  
PRINT N'Exception thrown, rolling back transaction.'  
ROLLBACK TRANSACTION  
PRINT N'Transaction rolled back.'   
END CATCH  
Go  
  
-- Clean up.  
DROP Procedure uspRollbackFromProc;  
Go  
DROP ASSEMBLY TestProcs;  
Go  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Transactions et l’intégration du CLR](../../relational-databases/clr-integration-data-access-transactions/clr-integration-and-transactions.md)  
  
  
