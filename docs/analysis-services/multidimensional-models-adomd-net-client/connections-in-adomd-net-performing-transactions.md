---
title: Exécution de Transactions dans ADOMD.NET | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: adomd
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: f9d433586e3feac9057eef165cb5d9f931532fdb
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="connections-in-adomdnet---performing-transactions"></a>Connexions dans ADOMD.NET - exécution de Transactions
  Dans ADOMD.NET, l'objet <xref:Microsoft.AnalysisServices.AdomdClient.AdomdTransaction> permet de gérer le contexte de transaction pour un objet <xref:Microsoft.AnalysisServices.AdomdClient.AdomdConnection> donné. Ces fonctionnalités vous permettent d'exécuter plusieurs commandes dans le même contexte. Chaque commande lit les mêmes données sans que les données lues ne soient modifiées entre chaque exécution de commande.  
  
> [!NOTE]  
>  Le <xref:Microsoft.AnalysisServices.AdomdClient.AdomdTransaction> classe est l’implémentation de la **System.Data.IDbTransaction** interface, une partie de la [!INCLUDE[msCoName](../../includes/msconame-md.md)] bibliothèque de classes .NET Framework et implémentée par tous les fournisseurs de données .NET Framework qui prennent en charge des transactions.  
  
 L'objet <xref:Microsoft.AnalysisServices.AdomdClient.AdomdTransaction> ne prend en charge que les transactions validées en lecture, dans lesquelles les verrous partagés sont maintenus pendant la lecture des données afin d'éviter des lectures erronées.  
  
 L'objet <xref:Microsoft.AnalysisServices.AdomdClient.AdomdConnection> est utilisée pour démarrer la transaction. Pour utiliser la transaction, des commandes sont ensuite exécutées sur la connexion qui a démarré la transaction. Une fois que vous avez terminé la transaction, vous pouvez soit l'annuler, soit la valider.  
  
## <a name="starting-a-transaction"></a>Démarrage d'une transaction  
 Vous pouvez créer une instance d'un objet <xref:Microsoft.AnalysisServices.AdomdClient.AdomdTransaction> en appelant la méthode <xref:Microsoft.AnalysisServices.AdomdClient.AdomdConnection.BeginTransaction%2A> de l'objet <xref:Microsoft.AnalysisServices.AdomdClient.AdomdConnection>. L'exemple suivant montre comment créer une instance de l'objet <xref:Microsoft.AnalysisServices.AdomdClient.AdomdTransaction> :  
  
```  
Dim objTransaction As AdomdTransaction = objConnection.BeginTransaction()  
AdomdTransaction objTransaction = objConnection.BeginTransaction();  
```  
  
## <a name="rolling-back-a-transaction"></a>Annulation d'une transaction  
 Pour annuler une transaction incomplète existante, vous devez appeler la méthode <xref:Microsoft.AnalysisServices.AdomdClient.AdomdTransaction.Rollback%2A> de l'objet <xref:Microsoft.AnalysisServices.AdomdClient.AdomdTransaction>. Si vous appelez cette méthode pour une transaction complète existante, une exception est levée.  
  
## <a name="committing-a-transaction"></a>Validation d'une transaction  
 Après avoir appelé la méthode <xref:Microsoft.AnalysisServices.AdomdClient.AdomdConnection.BeginTransaction%2A> pour démarrer une transaction, vous pouvez terminer la transaction en appelant la méthode <xref:Microsoft.AnalysisServices.AdomdClient.AdomdTransaction.Commit%2A> de l'objet <xref:Microsoft.AnalysisServices.AdomdClient.AdomdTransaction>. Si cette méthode est appelée pour une transaction complète existante, une exception est levée.  
  
## <a name="see-also"></a>Voir aussi  
 [Établissement de connexions dans ADOMD.NET](../../analysis-services/multidimensional-models-adomd-net-client/connections-in-adomd-net.md)   
 [Programmation du client ADOMD.NET](../../analysis-services/multidimensional-models-adomd-net-client/adomd-net-client-programming.md)  
  
  
