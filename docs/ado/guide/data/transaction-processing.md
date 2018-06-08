---
title: Traitement des transactions | Documents Microsoft
ms.prod: sql
ms.prod_service: connectivity
ms.component: ado
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.suite: sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- transactions [ADO]
- data updates [ADO], transaction processing
- updating data [ADO], transaction processing
- nested transactions [ADO]
ms.assetid: 74ab6706-e2dc-42cb-af77-dbc58a9cf4ce
caps.latest.revision: 12
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: b809f42f86646cff682127a6ce3836ab6ffaf095
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="transaction-processing"></a>Traitement des transactions
A *transaction* délimite le début et la fin d’une série d’opérations d’accès aux données exécutée sur une connexion. Selon les fonctionnalités transactionnelles de votre source de données, le **connexion** objet vous permet également de créer et gérer des transactions. Par exemple, à l’aide du fournisseur Microsoft OLE DB pour SQL Server pour accéder à une base de données Microsoft SQL Server, vous pouvez créer plusieurs transactions imbriquées pour les commandes que vous exécutez.  
  
 ADO garantit que les modifications apportées à une source de données résultant d’opérations dans une transaction de se produisent correctement ensemble ou pas du tout.  
  
 Si vous annulez la transaction, ou si un de ses opérations échoue, le résultat sera comme si aucune des opérations dans la transaction s’est produite. La source de données reste telle qu’elle était avant le début de la transaction.  
  
 ADO fournit les méthodes suivantes pour le contrôle des transactions : **BeginTrans**, **CommitTrans**, et **RollbackTrans**. Utilisez ces méthodes avec un **connexion** lorsque vous souhaitez enregistrer ou annuler une série de modifications apportées à la source de données comme une unité unique de l’objet. Par exemple, pour transférer de l’argent entre des comptes, soustraire un montant d’un et ajouter le volume à l’autre. Si une mise à jour échoue, les comptes ne sont pas équilibrés. Ces modifications dans une transaction ouverte garantit que toutes ou aucune des modifications passent par.  
  
> [!NOTE]
>  Tous les fournisseurs prennent en charge les transactions. Vérifiez que la propriété définie par le fournisseur «**Transaction DDL**» s’affiche dans le **connexion** l’objet [propriétés](../../../ado/reference/ado-api/properties-collection-ado.md) collection, indiquant que le fournisseur prend en charge transactions. Si le fournisseur ne prend pas en charge les transactions, appelant une de ces méthodes retourne une erreur.  
  
 Après avoir appelé la **BeginTrans** (méthode), le fournisseur n’est plus instantanément validera les modifications que vous apportez jusqu'à ce que vous appeliez **CommitTrans** ou **RollbackTrans** pour mettre fin à la transaction.  
  
 Appel de la **CommitTrans** méthode enregistre les modifications apportées dans une transaction ouverte sur la connexion et met fin à la transaction. Appel de la **RollbackTrans** méthode annule les modifications apportées dans une transaction ouverte et met fin à la transaction. Appel de ces méthodes lorsqu’il n’existe aucune transaction en cours génère une erreur.  
  
 Selon le **connexion** l’objet [attributs](../../../ado/reference/ado-api/attributes-property-ado.md) propriété, en appelant le **CommitTrans** ou **RollbackTrans** méthode peut Démarrer automatiquement une nouvelle transaction. Si le **attributs** est définie sur **adXactCommitRetaining**, le fournisseur lance automatiquement une nouvelle transaction après un **CommitTrans** appeler. Si le **attributs** est définie sur **adXactAbortRetaining**, le fournisseur lance automatiquement une nouvelle transaction après un **RollbackTrans** appeler.  
  
## <a name="transaction-isolation-level"></a>Niveau d’Isolation de transaction  
 Utilisez le **IsolationLevel** propriété à définir le niveau d’isolement d’une transaction sur une **connexion** objet. Le paramètre ne prendre effet qu’après la prochaine fois que vous appelez le [BeginTrans](../../../ado/reference/ado-api/begintrans-committrans-and-rollbacktrans-methods-ado.md) (méthode). Si le niveau d’isolation demandé n’est pas disponible, le fournisseur peut renvoyer le niveau d’isolation supérieur suivant. Reportez-vous à la **IsolationLevel** propriété de référence du programmeur ADO pour plus d’informations sur les valeurs valides.  
  
## <a name="nested-transactions"></a>Transactions imbriquées  
 Pour les fournisseurs qui prennent en charge les transactions imbriquées, l’appel du **BeginTrans** méthode dans une transaction ouverte démarre une nouvelle transaction imbriquée. La valeur de retour indique le niveau d’imbrication : une valeur de retour de « 1 » indique que vous avez ouvert une transaction de niveau supérieur (autrement dit, la transaction n'est pas imbriquée dans une autre transaction), « 2 » indique que vous avez ouvert une transaction de second niveau (une transaction imbriquée dans une transaction de niveau supérieur), et ainsi de suite. Appel de **CommitTrans** ou **RollbackTrans** affecte uniquement le plus récemment ouvert transaction ; vous devez fermer ou annuler la transaction en cours avant de résoudre toutes les transactions de niveau supérieur.
