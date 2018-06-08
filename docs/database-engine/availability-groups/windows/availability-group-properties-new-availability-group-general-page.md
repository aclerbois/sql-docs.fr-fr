---
title: 'Propriétés d’un groupe de disponibilité : Nouveau groupe de disponibilité (page Général) | Microsoft Docs'
ms.custom: ''
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: high-availability
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.swb.availabilitygroupproperties.general.f1
ms.assetid: 9af5379f-91b8-4729-9f75-4a80242a30e9
caps.latest.revision: 15
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: ae701b14ca047dba5b574e84407ae3ce459f9e44
ms.sourcegitcommit: 8aa151e3280eb6372bf95fab63ecbab9dd3f2e5e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34770855"
---
# <a name="availability-group-properties-new-availability-group-general-page"></a>Propriétés de groupe de disponibilité : nouveau groupe de disponibilité (page Général)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Cette rubrique s’applique à l’onglet **Général** de la boîte de dialogue **Nouveau groupe de disponibilité** et de la boîte de dialogue **Propriétés du groupe de disponibilité** .  La boîte de dialogue **Nouveau groupe de disponibilité** vous permet de créer un nouveau groupe de disponibilité sans utiliser l’ [!INCLUDE[ssAoNewAgWiz](../../../includes/ssaonewagwiz-md.md)]. La boîte de dialogue **Propriétés du groupe de disponibilité** vous permet d’afficher et de modifier la configuration d’un groupe de disponibilité existant.  
  
 **Pour afficher les propriétés d'un groupe de disponibilité**  
  
-   [Afficher les propriétés d’un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/view-availability-group-properties-sql-server.md)  
  
-   [Utiliser le tableau de bord Always On &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)  
  
## <a name="uielement-list"></a>Liste des éléments de l'interface utilisateur  
 **Nom du groupe de disponibilité**  
 Nom du groupe de disponibilité. Il s'agit d'un nom spécifié par l'utilisateur qui doit être unique dans le cluster de basculement Windows Server (WSFC).  
  
## <a name="availability-databases"></a>Bases de données de disponibilité  
 **Database Name**  
 Nom d'une base de données qui a été ajoutée au groupe de disponibilité.  
  
 **Ajouter**  
 Cliquez pour ajouter une base de données au groupe de disponibilité.  
  
 **Supprimer**  
 Cliquez pour supprimer une base de données sélectionnée du groupe de disponibilité.  
  
## <a name="availability-replicas"></a>Réplicas de disponibilité  
 **Instance de serveur**  
 Nom de serveur de l'instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] qui héberge ce réplica et, pour une instance non définie par défaut, son nom d'instance.  
  
 **Rôle**  
 **Principal**  
 Actuellement le réplica principal.  
  
 **Secondary**  
 Actuellement un réplica secondaire.  
  
 **Résolution**  
 Actuellement le rôle de réplica est en cours d'être résolu en rôle principal ou secondaire.  
  
 **Mode de disponibilité**  
 Mode de disponibilité du réplica, parmi :  
  
 **Validation asynchrone**  
 Le réplica principal peut valider des transactions sans attendre que le réplica secondaire écrive le journal sur le disque.  
  
 **Validation synchrone**  
 Le réplica principal attend pour valider une transaction donnée que le réplica secondaire ait écrit la transaction sur le disque.  
  
 Pour plus d’informations, consultez [Modes de disponibilité &#40;groupes de disponibilité Always On&#41;](../../../database-engine/availability-groups/windows/availability-modes-always-on-availability-groups.md).  
  
 **Mode de basculement**  
 Mode de basculement du réplica, parmi :  
  
 **Automatic**  
 Basculement automatique. Le réplica est une cible pour les basculements automatiques. Pris en charge uniquement si le mode de disponibilité est défini en mode de validation synchrone.  
  
 **Manuel**  
 Basculement manuel. Le réplica peut uniquement être basculé manuellement par l'administrateur de base de données.  
  
 **Connexions dans le rôle principal**  
 Type de connexions clientes pris en charge lorsque le réplica joue le rôle principal.  
  
 **Autoriser toutes les connexions**  
 Toutes les connexions aux bases de données sont autorisées dans le réplica principal. Il s'agit du paramètre par défaut.  
  
 **Autoriser les connexions en lecture/écriture**  
 Les connexions où la propriété de connexion d’intention de l’application a la valeur **ReadOnly** ne sont pas autorisées. Lorsque la propriété d'intention de l'application a la valeur **ReadWrite** ou si cette propriété n'est pas définie, la connexion est autorisée. Pour plus d'informations sur la propriété de connexion d'intention de l'application, consultez [Using Connection String Keywords with SQL Server Native Client](../../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md).  
  
 **Lisible secondaire**  
 Si un réplica de disponibilité qui revêt le rôle secondaire (autrement dit, un réplica secondaire) peut accepter les connexions des clients, les options suivantes sont disponibles :  
  
 **Non**  
 Aucune connexion directe n'est autorisée aux bases de données secondaires de ce réplica. Elles ne sont pas disponibles pour l'accès en lecture. Il s'agit du paramètre par défaut.  
  
 **Intention de lecture uniquement**  
 Seules les connexions en lecture seule directes sont autorisées aux bases de données secondaires de ce réplica. La ou les bases de données secondaires sont toutes disponibles pour l'accès en lecture.  
  
 **Oui**  
 Toutes les connexions sont autorisées aux bases de données secondaires de ce réplica, mais uniquement pour l'accès en lecture. La ou les bases de données secondaires sont toutes disponibles pour l'accès en lecture.  
  
 **Délai d'attente de session (secondes)**  
 Nombre de secondes pour la période de délai d'expiration de session sur ce réplica.  
  
 **URL de point de terminaison**  
 URL du point de terminaison. Pour plus d’informations sur le format de ces URL, consultez [Spécifier l’URL de point de terminaison lors de l’ajout ou lors de la modification d’un réplica de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/specify-endpoint-url-adding-or-modifying-availability-replica.md).  
  
 **Ajouter**  
 Cliquez pour ajouter un réplica secondaire au groupe de disponibilité.  
  
 **Supprimer**  
 Cliquez pour supprimer un réplica secondaire du groupe de disponibilité.  
  
## <a name="see-also"></a> Voir aussi  
 [Vue d’ensemble des groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)  
  
  
