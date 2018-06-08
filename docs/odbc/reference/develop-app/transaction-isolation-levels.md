---
title: Niveaux d’isolement | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- dirty reads [ODBC]
- isolation levels [ODBC]
- nonrepeatable reads [ODBC]
- read uncommitted [ODBC]
- read committed [ODBC]
- serializable reads [ODBC]
- phantoms [ODBC]
- transaction isolation [ODBC]
- repeatable reads [ODBC]
- transactions [ODBC], isolation
ms.assetid: 0d638d55-ffd0-48fb-834b-406f466214d4
caps.latest.revision: 6
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 2dc2b07938b8100128eaf8433aa4ab48002c357d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="transaction-isolation-levels"></a>Niveaux d'isolement des transactions
*Niveaux d’isolation des transactions* sont une mesure de l’étendue pour la transaction isolation aboutit. En particulier, les niveaux d’isolation des transactions sont définis par la présence ou l’absence de phénomènes suivants :  
  
-   **Dirty lectures** A *lecture erronée* se produit lorsqu’une transaction lit les données qui n’a pas encore été validées. Par exemple, mises à jour de la transaction 1 une ligne. La transaction 2 lit la ligne mise à jour avant de la transaction 1 valide de la mise à jour. Si la transaction 1 annule la modification, la transaction 2 ont lira des données qui sont considéré comme jamais existé.  
  
-   **Lectures non reproductibles** A *lecture non renouvelable* se produit lorsqu’une transaction lit la même ligne à deux reprises, mais Obtient des données différentes à chaque fois. Par exemple, la transaction 1 lit une ligne. La transaction 2 met à jour ou supprime cette ligne et valide la mise à jour ou la suppression. Si la transaction 1 permet de relire la ligne, il récupère les valeurs de ligne différente ou découvre que la ligne a été supprimée.  
  
-   **Fantômes** A *fantôme* est une ligne qui correspond aux critères de recherche, mais n’est pas visible initialement. Par exemple, supposons que la transaction 1 lit un ensemble de lignes qui répondent à certains critères de recherche. La transaction 2 génère une nouvelle ligne (via une mise à jour ou une instruction insert) qui correspond aux critères de recherche pour la transaction 1. Si la transaction 1 reexecutes l’instruction qui lit les lignes, il obtient un ensemble de lignes différent.  
  
 Les niveaux d’isolation de quatre transaction (telle que définie par SQL-92) sont définis en termes de ces phénomènes. Dans le tableau suivant, un « X » marque chaque phénomène peut se produire.  
  
|Niveau d’isolation de transaction|Lectures de données modifiées|Lectures non reproductibles|Fantômes|  
|---------------------------------|-----------------|-------------------------|--------------|  
|Lecture non validée|X|X|X|  
|Lecture validée|--|X|X|  
|Lecture renouvelable|--|--|X|  
|Sérialisable|--|--|--|  
  
 Le tableau suivant décrit les méthodes simples qu’un SGBD peut implémenter les niveaux d’isolation des transactions.  
  
> [!IMPORTANT]  
>  La plupart des SGBD utilisent des schémas plus complexes que ces pour accroître la simultanéité. Ces exemples sont fournis à titre d’illustration uniquement. En particulier, ODBC n’impose pas de SGBD particulier comment isoler les transactions entre eux.  
  
|Isolement des transactions|Implémentation possible|  
|---------------------------|-----------------------------|  
|Lecture non validée|Les transactions ne sont pas isolées des autres. Si le SGBD prend en charge les autres niveaux d’isolation de transaction, il ignore tout mécanisme qu’il utilise pour implémenter ces niveaux. Afin que leur n’altèrent pas d’autres transactions, les transactions en cours d’exécution au niveau Read Uncommitted sont généralement en lecture seule.|  
|Lecture validée|La transaction attend jusqu'à ce que les lignes verrouillées en écriture par d’autres transactions sont déverrouillées ; Cela empêche de lire des données modifiées (« dirty ».<br /><br /> La transaction contient un verrou de lecture (si elle ne lit la ligne) ou l’écriture de verrouillage (si elle met à jour ou supprime la ligne) sur la ligne actuelle pour empêcher d’autres transactions à partir de la mise à jour ou de le supprimer. La transaction libère le verrou en lecture lorsqu’il se déplace hors de la ligne actuelle. Il maintient un verrou d’écriture jusqu'à ce qu’elle est validée ou restaurée.|  
|Lecture renouvelable|La transaction attend jusqu'à ce que les lignes verrouillées en écriture par d’autres transactions sont déverrouillées ; Cela empêche de lire des données modifiées (« dirty ».<br /><br /> La transaction maintient un verrou de lecture sur toutes les lignes, qu'elle retourne des verrous de l’application et en écriture sur les lignes de toutes les insertions, mises à jour, ou supprime. Par exemple, si la transaction inclut l’instruction SQL **sélectionnez \* de commandes**, comme l’application extrait les lignes des transaction des verrouillages en lecture. Si la transaction inclut l’instruction SQL **supprimer à partir de commandes où état = 'Fermé'**, les verrous d’écriture transaction lignes comme il les supprime.<br /><br /> Étant donné que les autres transactions ne peut pas mettre à jour ou supprimer ces lignes, la transaction actuelle permet d’éviter des lectures non reproductibles. La transaction libère ses verrous lorsqu’elle est validée ou restaurée.|  
|Sérialisable|La transaction attend jusqu'à ce que les lignes verrouillées en écriture par d’autres transactions sont déverrouillées ; Cela empêche de lire des données modifiées (« dirty ».<br /><br /> La transaction maintient un verrou de lecture (si elle lit uniquement les lignes) ou le verrou d’écriture (s’il peut mettre à jour ou supprimer des lignes) de la plage de lignes qu’il affecte. Par exemple, si la transaction inclut l’instruction SQL **sélectionnez \* de commandes**, la plage est l’ensemble de table Orders ; les transaction des verrouillages en lecture la table et ne pas autoriser toutes les nouvelles lignes à insérer dans ce. Si la transaction inclut l’instruction SQL **supprimer à partir de commandes où état = 'Fermé'**, la plage est toutes les lignes avec un état de « CLOSED » ; les verrous d’écriture transaction toutes les lignes de commandes de table avec l’état « CLOSED » et n’autorise pas les lignes à insérer ou la mise à jour la ligne qui en résulte a le statut « Fermé ».<br /><br /> Étant donné que les autres transactions ne peut pas mettre à jour ou supprimer les lignes de la plage, la transaction actuelle permet d’éviter des lectures non reproductibles. Étant donné que les autres transactions ne peut pas insérer des lignes dans la plage, la transaction actuelle évite toute fantômes. La transaction libère son verrou lorsqu’elle est validée ou restaurée.|  
  
 Il est important de noter que le niveau d’isolation de transaction n’affecte pas la possibilité d’une transaction pour voir ses propres modifications ; les transactions peuvent toujours voir les modifications. Par exemple, une transaction peut être constituée de deux **mise à jour** instructions, la première qui déclenche le salaire de tous les employés de 10 % et le second définit le salaire de tous les employés sur certains quantité maximale de ce montant. Cette opération réussit comme une transaction unique uniquement parce que la seconde **mise à jour** instruction permettre voir les résultats de la première.
