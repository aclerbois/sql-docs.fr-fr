---
title: Comparer des tables répliquées pour identifier les différences (programmation de réplication) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: replication
ms.reviewer: ''
ms.suite: sql
ms.technology:
- replication
ms.tgt_pltfrm: ''
ms.topic: conceptual
dev_langs:
- TSQL
helpviewer_keywords:
- tablediff utility
- comparing replicated tables
ms.assetid: cd253a17-0c85-42b4-912c-690169ebe799
caps.latest.revision: 20
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 4217b463cedd805729dc2f99a4110feffa5573c0
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="compare-replicated-tables-for-differences-replication-programming"></a>Comparer des tables répliquées pour identifier les différences (programmation de réplication)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  La validation d'article est utilisée pour déterminer si les données publiées pour les articles de table sur le serveur de publication et sur l'Abonné ne sont pas identiques, ce qui peut indiquer une non-convergence. Pour plus d’informations, consultez [Valider des données répliquées](../../../relational-databases/replication/validate-replicated-data.md). Toutefois, la validation retourne uniquement des informations de succès ou d'échec et ne fournit pas d'informations sur les différences entre les tables sources et les tables cibles. L’utilitaire d’invite de commandes **tablediff** retourne des informations détaillées sur les différences entre les deux tables et peut même générer un script [!INCLUDE[tsql](../../../includes/tsql-md.md)] pour établir la convergence de l’abonnement avec les données sur le serveur de publication.  
  
> [!NOTE]  
>  L’utilitaire **tablediff** est pris en charge uniquement pour les serveurs [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
### <a name="to-compare-replicated-tables-for-differences-using-tablediff"></a>Pour comparer les différences des tables répliquées à l'aide de tablediff  
  
1.  À partir de l'invite de commandes sur un serveur quelconque d'une topologie de réplication, exécutez l' [tablediff Utility](../../../tools/tablediff-utility.md). Spécifiez les paramètres suivants :  
  
    -   **-sourceserver** - nom du serveur sur lequel les données sont reconnues comme correctes, habituellement le serveur de publication.  
  
    -   **-sourcedatabase** - nom de la base de données contenant les données correctes.  
  
    -   **-sourcetable** - nom de la table source pour l'article comparé.  
  
    -   (Facultatif) **-sourceschema** - propriétaire du schéma de la table source, sinon le schéma par défaut.  
  
    -   (Facultatif) **-sourceuser** et **-sourcepassword** en cas d'utilisation de l'authentification SQL Server pour se connecter au serveur de publication.  
  
        > [!IMPORTANT]  
        >  Lorsque c'est possible, utilisez l'authentification Windows. Si vous devez utiliser l'authentification [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , invitez les utilisateurs à entrer les informations d'identification de sécurité pendant l'exécution. Si vous devez enregistrer les informations d'identification dans un fichier de script, vous devez sécuriser le fichier pour empêcher un accès non autorisé.  
  
    -   **-destinationserver** - nom du serveur sur lequel les données sont comparées, habituellement un Abonné.  
  
    -   **-destinationdatabase** - nom de la base de données en cours de comparaison.  
  
    -   **-destinationtable** - nom de la table qui est comparée.  
  
    -   (Facultatif) **-destinationschema** - propriétaire du schéma de la table cible, sinon le schéma par défaut.  
  
    -   (Facultatif) **-destinationuser** et **-destinationpassword** en cas d'utilisation de l'authentification [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pour se connecter à l'Abonné.  
  
        > [!IMPORTANT]  
        >  Lorsque c'est possible, utilisez l'authentification Windows. Si vous devez utiliser l'authentification [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , invitez les utilisateurs à entrer les informations d'identification de sécurité pendant l'exécution. Si vous devez enregistrer les informations d'identification dans un fichier de script, vous devez sécuriser le fichier pour empêcher un accès non autorisé.  
  
    -   (Facultatif) Utilisez **-c** pour faire une comparaison au niveau des colonnes.  
  
    -   (Facultatif) Utilisez **- q** pour faire une comparaison rapide du nombre de lignes et du schéma uniquement.  
  
    -   (Facultatif) Spécifiez un nom de fichier et un chemin d'accès pour **-o** pour générer la sortie des résultats dans un fichier.  
  
    -   (Facultatif) Spécifiez une table de la base de données d'abonnement dans laquelle insérer les résultats pour **-et**. Si la table existe déjà, spécifiez **-dt** pour supprimer d'abord la table.  
  
    -   (Facultatif) Utilisez **-f** pour générer un fichier [!INCLUDE[tsql](../../../includes/tsql-md.md)] et corriger les données sur l'Abonné afin qu'elles correspondent à celles du serveur de publication. Utilisez **-df** pour spécifier le nombre d'instructions [!INCLUDE[tsql](../../../includes/tsql-md.md)] dans chaque fichier.  
  
    -   (Facultatif) Utilisez **-rc** et **-ri** pour spécifier le nombre de tentatives autorisées pour une opération et l'intervalle avant chaque nouvelle tentative.  
  
    -   (Facultatif) Utilisez **-strict** pour mettre en vigueur la comparaison stricte de schémas entre les tables sources et les tables cibles.  
  
## <a name="see-also"></a> Voir aussi  
 [Valider des données sur l’abonné](../../../relational-databases/replication/validate-data-at-the-subscriber.md)  
  
  
