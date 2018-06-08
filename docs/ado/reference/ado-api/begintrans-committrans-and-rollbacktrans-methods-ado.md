---
title: BeginTrans, CommitTrans et RollbackTrans, méthodes (ADO) | Documents Microsoft
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
apitype: COM
f1_keywords:
- Connection15::raw_RollbackTrans
- Connection15::CommitTrans
- Connection15::raw_CommitTrans
- Connection15::raw_BeginTrans
- Connection15::BeginTrans
- Connection15::RollbackTrans
helpviewer_keywords:
- BeginTrans method [ADO]
- CommitTrans method [ADO]
- RollbackTrans method [ADO]
ms.assetid: d4683472-4120-4236-8640-fa9ae289e23e
caps.latest.revision: 12
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 80ae8b47737573aa7ff0c81bafd882ca162a157f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="begintrans-committrans-and-rollbacktrans-methods-ado"></a>BeginTrans, CommitTrans et RollbackTrans, méthodes (ADO)
Ces méthodes de transaction gèrent le traitement des transactions dans un [connexion](../../../ado/reference/ado-api/connection-object-ado.md) de l’objet comme suit :  
  
-   **BeginTrans** commence une nouvelle transaction.  
  
-   **CommitTrans** enregistre les modifications et termine la transaction actuelle. Il peut également démarrer une nouvelle transaction.  
  
-   **RollbackTrans** annule les modifications apportées pendant la transaction active et termine la transaction. Il peut également démarrer une nouvelle transaction.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
level = object.BeginTrans()  
object.BeginTrans  
object.CommitTrans  
object.RollbackTrans  
```  
  
## <a name="return-value"></a>Valeur retournée  
 **BeginTrans** peut être appelé comme une fonction qui retourne un **Long** variable indiquant le niveau d’imbrication de la transaction.  
  
#### <a name="parameters"></a>Paramètres  
 *objet*  
 A **connexion** objet.  
  
## <a name="connection"></a>Connexion  
 Utilisez ces méthodes avec un **connexion** lorsque vous souhaitez enregistrer ou annuler une série de modifications apportées à la source de données comme une unité unique de l’objet. Par exemple, pour transférer de l’argent entre des comptes, soustraire un montant d’un et ajouter le volume à l’autre. Si une mise à jour échoue, les comptes ne sont pas équilibrés. Ces modifications dans une transaction ouverte garantit que toutes ou aucune des modifications passent par.  
  
> [!NOTE]
>  Tous les fournisseurs prennent en charge les transactions. Vérifiez que la propriété définie par le fournisseur «**Transaction DDL**» s’affiche dans le **connexion** l’objet [propriétés](../../../ado/reference/ado-api/properties-collection-ado.md) collection, indiquant que le fournisseur prend en charge transactions. Si le fournisseur ne prend pas en charge les transactions, appelant une de ces méthodes retourne une erreur.  
  
 Après avoir appelé la **BeginTrans** (méthode), le fournisseur n’est plus instantanément validera les modifications que vous apportez jusqu'à ce que vous appeliez **CommitTrans** ou **RollbackTrans** pour mettre fin à la transaction.  
  
 Pour les fournisseurs qui prennent en charge les transactions imbriquées, l’appel du **BeginTrans** méthode dans une transaction ouverte démarre une nouvelle transaction imbriquée. La valeur de retour indique le niveau d’imbrication : une valeur de retour de « 1 » indique que vous avez ouvert une transaction de niveau supérieur (autrement dit, la transaction n'est pas imbriquée dans une autre transaction), « 2 » indique que vous avez ouvert une transaction de second niveau (une transaction imbriquée dans une transaction de niveau supérieur), et ainsi de suite. Appel de **CommitTrans** ou **RollbackTrans** affecte uniquement le plus récemment ouvert transaction ; vous devez fermer ou annuler la transaction en cours avant de résoudre toutes les transactions de niveau supérieur.  
  
 Appel de la **CommitTrans** méthode enregistre les modifications apportées dans une transaction ouverte sur la connexion et met fin à la transaction. Appel de la **RollbackTrans** méthode annule les modifications apportées dans une transaction ouverte et met fin à la transaction. Appel de ces méthodes lorsqu’il n’existe aucune transaction en cours génère une erreur.  
  
 Selon le **connexion** l’objet [attributs](../../../ado/reference/ado-api/attributes-property-ado.md) propriété, en appelant le **CommitTrans** ou **RollbackTrans** méthodes peuvent Démarrer automatiquement une nouvelle transaction. Si le **attributs** est définie sur **adXactCommitRetaining**, le fournisseur lance automatiquement une nouvelle transaction après un **CommitTrans** appeler. Si le **attributs** est définie sur **adXactAbortRetaining**, le fournisseur lance automatiquement une nouvelle transaction après un **RollbackTrans** appeler.  
  
## <a name="remote-data-service"></a>Service de données à distance  
 Le **BeginTrans**, **CommitTrans**, et **RollbackTrans** méthodes ne sont pas disponibles sur un côté client **connexion** objet.  
  
## <a name="applies-to"></a>S'applique à  
 [Connection, objet (ADO MD)](../../../ado/reference/ado-api/connection-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [BeginTrans, CommitTrans et RollbackTrans, méthodes-exemple (VB)](../../../ado/reference/ado-api/begintrans-committrans-and-rollbacktrans-methods-example-vb.md)   
 [BeginTrans, CommitTrans et RollbackTrans, méthodes-exemple (VC ++)](../../../ado/reference/ado-api/begintrans-committrans-and-rollbacktrans-methods-example-vc.md)   
 [Attributes, propriété (ADO)](../../../ado/reference/ado-api/attributes-property-ado.md)
