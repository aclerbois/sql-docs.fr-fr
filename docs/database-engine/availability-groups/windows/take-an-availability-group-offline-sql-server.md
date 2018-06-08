---
title: Placer un groupe de disponibilité hors connexion (SQL Server) | Microsoft Docs
ms.custom: ''
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: high-availability
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], take offline
ms.assetid: 50f5aad8-0dff-45ef-8350-f9596d3db898
caps.latest.revision: 38
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 80e9347c8776b4f2bbbbc061f4e6e96ab2135cb9
ms.sourcegitcommit: 8aa151e3280eb6372bf95fab63ecbab9dd3f2e5e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34770695"
---
# <a name="take-an-availability-group-offline-sql-server"></a>Placer un groupe de disponibilité hors connexion (SQL Server)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Cette rubrique explique comment faire passer un groupe de disponibilité Always On de l’état ONLINE à l’état OFFLINE à l’aide de [!INCLUDE[tsql](../../../includes/tsql-md.md)] dans [!INCLUDE[ssSQL11SP1](../../../includes/sssql11sp1-md.md)] et versions ultérieures. Il n'y a aucune perte de données des bases de données de validation synchrone, car si aucun réplica avec validation synchrone n'est synchronisé, l'opération OFFLINE génère une erreur et conserve le groupe de disponibilité dans l'état ONLINE. Conserver le groupe de disponibilité en ligne protège les bases de données non synchronisées avec validation synchrone contre la perte de données. Après qu'un groupe de disponibilité a été mis hors connexion, ses bases de données deviennent indisponibles pour les clients et vous ne pouvez pas remettre le groupe de disponibilité en ligne. Par conséquent, mettez un groupe de disponibilité hors connexion uniquement pour migrer les ressources du groupe de disponibilité d'un cluster WSFC à un autre.  
  
 Pendant une migration entre clusters de [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], si les applications se connectent directement au réplica principal d'un groupe de disponibilité, le groupe de disponibilité doit être mis hors connexion. La migration entre clusters de [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] prend en charge la mise à niveau du système d'exploitation avec un temps mort minimal des groupes de disponibilité. Le scénario classique consiste à utiliser la migration entre clusters de [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] pour la mise à niveau du système d'exploitation vers [!INCLUDE[win8](../../../includes/win8-md.md)] ou [!INCLUDE[win8srv](../../../includes/win8srv-md.md)]. Pour plus d’informations, consultez [Cross-Cluster Migration of Always On Availability Groups for OS Upgrade](http://msdn.microsoft.com/library/jj873730.aspx)(Migration entre clusters de groupes de disponibilité Always On pour la mise à niveau du système d’exploitation).  
  
-   **Avant de commencer :**  
  
     [Conditions préalables](#Prerequisites)  
  
     [Recommandations](#Recommendations)  
  
     [Sécurité](#Security)  
  
-   **Pour mettre un groupe de disponibilité hors connexion, utilisez :**  [Transact-SQL](#TsqlProcedure)  
  
-   **Suivi :**  [Après que le groupe de disponibilité a été mis hors connexion](#FollowUp)  
  
-   [Contenu connexe](#RelatedContent)  
  
##  <a name="BeforeYouBegin"></a> Avant de commencer  
  
> [!CAUTION]  
>  Utilisez l'option OFFLINE uniquement pour une migration entre clusters des ressources du groupe de disponibilité.  
  
###  <a name="Prerequisites"></a> Conditions préalables  
  
-   L'instance de serveur sur laquelle vous sélectionnez la commande OFFLINE doit exécuter [!INCLUDE[ssSQL11SP1](../../../includes/sssql11sp1-md.md)] ou version ultérieure (édition Entreprise ou ultérieure).  
  
-   Le groupe de disponibilité doit être en ligne.  
  
###  <a name="Recommendations"></a> Recommandations  
 Avant de mettre le groupe de disponibilité hors connexion, supprimez le ou les écouteurs du groupe de disponibilité. Pour plus d’informations, consultez [Supprimer un écouteur de groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remove-an-availability-group-listener-sql-server.md).  
  
###  <a name="Security"></a> Sécurité  
  
####  <a name="Permissions"></a> Permissions  
 Requiert l'autorisation ALTER AVAILABILITY GROUP sur le groupe de disponibilité, l'autorisation CONTROL AVAILABILITY GROUP, l'autorisation ALTER ANY AVAILABILITY GROUP ou l'autorisation CONTROL SERVER.  
  
##  <a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
 **Pour placer le groupe de disponibilité hors connexion**  
  
1.  Connectez-vous à une instance de serveur qui héberge un réplica de disponibilité pour le groupe de disponibilité. Ce réplica peut être le réplica principal ou un réplica secondaire.  
  
2.  Utilisez l'instruction [ALTER AVAILABILITY GROUP](../../../t-sql/statements/alter-availability-group-transact-sql.md) , comme suit :  
  
     ALTER AVAILABILITY GROUP *nom_groupe* OFFLINE  
  
     où *nom_groupe* correspond au nom du groupe de disponibilité.  
  
### <a name="example"></a> Exemple  
 L'exemple suivant place le groupe de disponibilité `AccountsAG` hors connexion.  
  
```  
ALTER AVAILABILITY GROUP AccountsAG OFFLINE;  
```  
  
##  <a name="FollowUp"></a> Suivi : Après que le groupe de disponibilité a été mis hors connexion  
  
-   **Enregistrement d l'opération OFFLINE :**  l'identité du nœud WSFC sur lequel l'opération OFFLINE a été initialisée est stockée dans le journal du cluster WSFC et dans le journal des erreurs SQL.  
  
-   **Si vous n’avez pas supprimé l’écouteur du groupe de disponibilité avant la mise hors connexion du groupe :**  si vous migrez le groupe de disponibilité vers un autre cluster WSFC, supprimez le numéro de réseau virtuel et l’adresse IP virtuelle de l’écouteur. Vous pouvez les supprimer à l’aide de la console de gestion du cluster de basculement, de l’applet de commande PowerShell [Remove-ClusterResource](http://technet.microsoft.com/library/ee461015\(WS.10\).aspx) ou de [cluster.exe](http://technet.microsoft.com/library/ee461015\(WS.10\).aspx). Notez que cluster.exe est déconseillé sur Windows 8.  
  
##  <a name="RelatedTasks"></a> Tâches associées  
  
-   [Supprimer un écouteur de groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remove-an-availability-group-listener-sql-server.md)  
  
-   [Changer le contexte de cluster HADR de l’instance de serveur &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/change-the-hadr-cluster-context-of-server-instance-sql-server.md)  
  
##  <a name="RelatedContent"></a> Contenu associé  
  
-   [Articles techniques SQL Server 2012](http://msdn.microsoft.com/library/bb418445\(SQL.10\).aspx)  
  
-   [Blog de l’équipe de SQL Server Always On : Blog officiel de l’équipe de SQL Server Always On](https://blogs.msdn.microsoft.com/sqlalwayson/)  
  
## <a name="see-also"></a> Voir aussi  
 [Groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)  
  
  
