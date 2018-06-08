---
title: Utilisation de System.Transactions | Documents Microsoft
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
dev_langs:
- VB
- CSharp
helpviewer_keywords:
- TransactionScope class
- Dispose method
- System.Transactions namespace
ms.assetid: 79656ce5-ce46-4c5e-9540-cf9869bd774b
caps.latest.revision: 16
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: b6a16064cbff2b859f627271784d170b0c812078
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="using-systemtransactions"></a>Utilisation de System.Transactions
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  L'espace de noms **System.Transactions** fournit une infrastructure de transaction qui s'intègre entièrement à ADO.NET et au CLR (Common Language Runtime) de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . La classe **System.Transactions.TransactionScope** crée un bloc de code transactionnel en inscrivant implicitement les connexions dans une transaction distribuée. Vous devez appeler la méthode **Complete** à la fin du bloc de code marqué par **TransactionScope**. La méthode **Dispose** est appelée lorsque l'exécution du programme laisse un bloc de code, ce qui provoque une interruption de la transaction si la méthode **Complete** n'est pas appelée. Si une exception a été levée qui provoque l'abandon de l'étendue par le code, la transaction est considérée comme supprimée.  
  
 Nous vous recommandons d'employer un bloc **using** pour garantir que la méthode **Dispose** est appelée sur l'objet **TransactionScope** à la sortie du bloc **using** . L'impossibilité de valider ou d'annuler les transactions en attente peut dégrader sérieusement les performances parce que le délai d'expiration par défaut de **TransactionScope** est une minute. Si vous n'utilisez pas une instruction **using** , vous devez effectuer tout le travail dans un bloc **Try** et appeler explicitement la méthode **Dispose** dans le bloc **Finally** .  
  
 Si une exception se produit dans **TransactionScope**, la transaction est marquée comme non cohérente et est abandonnée. Elle est annulée quand **TransactionScope** est supprimé. Si aucune exception ne se produit, les transactions sont validées.  
  
 **TransactionScope** doit être utilisé uniquement en cas d'accès des sources de données locales et distantes ou des gestionnaires de ressources externes. La raison en est que **TransactionScope** provoque toujours une promotion de la transaction, même si elle n'est utilisé que dans une connexion de contexte.  
  
> [!NOTE]  
>  La classe **TransactionScope** crée par défaut une transaction avec un **System.Transactions.Transaction.IsolationLevel** ayant la valeur **Serializable** . Selon votre application, vous pouvez envisager de baisser le niveau d'isolation pour éviter une contention élevée au sein de votre application.  
  
> [!NOTE]  
>  Nous vous recommandons de n'effectuer que des mises à jour, des insertions et des suppressions dans les transactions distribuées sur des serveurs distants parce qu'ils consomment d'importantes ressources de base de données. Si l'opération est effectuée sur le serveur local, une transaction distribuée n'est pas nécessaire et une transaction locale suffit. Les instructions SELECT peuvent verrouiller les ressources de base de données inutilement et, dans certains scénarios, il peut être nécessaire d'utiliser des transactions pour les sélections. Tout travail autre que celui sur une base de données doit être exécuté en dehors de la portée de la transaction, à moins qu'il n'implique d'autres gestionnaires de ressources transactionnels. Bien qu'une exception dans la portée de la transaction empêche la transaction d'être validée, la classe **TransactionScope** n'a aucune provision pour annuler les modifications apportées par votre code à l'extérieur de la portée de la transaction elle-même. Si une mesure doit être prise lorsque la transaction est annulée, vous devez écrire votre propre implémentation de l'interface **System.Transactions.IEnlistmentNotification** et l'inscrire explicitement dans la transaction.  
  
## <a name="example"></a>Exemple  
 Pour utiliser **System.Transactions**, vous devez avoir une référence au fichier System.Transactions.dll.  
  
 Le code suivant montre comment créer une transaction qui peut être promue par rapport à deux instances différentes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Ces instances sont représentées par deux objets **System.Data.SqlClient.SqlConnection** différents, encapsulés dans un bloc **TransactionScope** . Le code crée le bloc **TransactionScope** avec une instruction **using** et ouvre la première connexion, qui automatiquement l'inscrit dans **TransactionScope**. La transaction est inscrite initialement comme transaction légère, et non comme transaction distribuée complète. Le code présume l'existence d'une logique conditionnelle (laquelle a été omise pour des raisons de concision). Il ouvre la deuxième connexion uniquement si nécessaire, en l'inscrivant dans **TransactionScope**. Lorsque la connexion est ouverte, la transaction est automatiquement promue en transaction distribuée complète. Le code appelle ensuite **TransactionScope.Complete**, qui valide la transaction. Le code supprime les deux connexions à la sortie des instructions **using** . La méthode **TransactionScope.Dispose** pour le **TransactionScope** est appelée automatiquement à la fin du bloc **using** pour le **TransactionScope**. Si une exception a été levée à un point quelconque du bloc **TransactionScope** , l'appel de **Complete** n'a pas lieu et la transaction distribuée est annulée quand **TransactionScope** est supprimé.  
  
 Visual Basic  
  
```  
Using transScope As New TransactionScope()  
    Using connection1 As New SqlConnection(connectString1)  
        ' Opening connection1 automatically enlists it in the   
        ' TransactionScope as a lightweight transaction.  
        connection1.Open()  
  
        ' Do work in the first connection.  
  
        ' Assumes conditional logic in place where the second  
        ' connection will only be opened as needed.  
        Using connection2 As New SqlConnection(connectString2)  
            ' Open the second connection, which enlists the   
            ' second connection and promotes the transaction to  
            ' a full distributed transaction.  
            connection2.Open()  
  
            ' Do work in the second connection.  
  
        End Using  
    End Using  
  
    ' Commit the transaction.  
    transScope.Complete()  
End Using  
```  
  
 C#  
  
```  
using (TransactionScope transScope = new TransactionScope())  
{  
    using (SqlConnection connection1 = new   
       SqlConnection(connectString1))  
    {  
        // Opening connection1 automatically enlists it in the   
        // TransactionScope as a lightweight transaction.  
        connection1.Open();  
  
        // Do work in the first connection.  
  
        // Assumes conditional logic in place where the second  
        // connection will only be opened as needed.  
        using (SqlConnection connection2 = new   
            SqlConnection(connectString2))  
        {  
            // Open the second connection, which enlists the   
            // second connection and promotes the transaction to  
            // a full distributed transaction.   
            connection2.Open();  
  
            // Do work in the second connection.  
        }  
    }  
    //  The Complete method commits the transaction.  
    transScope.Complete();  
}  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Transactions et l’intégration du CLR](../../relational-databases/clr-integration-data-access-transactions/clr-integration-and-transactions.md)  
  
  
