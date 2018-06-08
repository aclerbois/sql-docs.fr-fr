---
title: Configurer les paramètres de propriété FailureConditionLevel | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: high-availability
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 513dd179-9a46-46da-9fdd-7632cf6d0816
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 0197b89a1109a1a834b2bb285da3ac4e71a9da21
ms.sourcegitcommit: 8aa151e3280eb6372bf95fab63ecbab9dd3f2e5e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34772385"
---
# <a name="configure-failureconditionlevel-property-settings"></a>Configurer les paramètres de propriété FailureConditionLevel
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Utilisez la propriété FailureConditionLevel pour définir les conditions de basculement ou de redémarrage de l’instance de cluster de basculement (FCI) Always On. Les modifications de cette propriété sont appliquées immédiatement sans nécessiter le redémarrage du service WSFC (cluster de basculement Windows Server) ou de la ressource FCI.  
  
-   **Avant de commencer :**  [Paramètres de propriété FailureConditionLevel](#Restrictions), [Sécurité](#Security)  
  
-   **Pour configurer les paramètres de propriété FailureConditionLevel à l’aide de** [PowerShell](#PowerShellProcedure), [Gestionnaire du cluster de basculement](#WSFC), [Transact-SQL](#TsqlProcedure)  
  
##  <a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="Restrictions"></a> Paramètres de propriété FailureConditionLevel  
 Les conditions d'échec sont définies sur une échelle croissante. Pour les niveaux 1-5, chaque niveau inclut toutes les conditions des niveaux précédents en plus de ses propres conditions. Cela signifie qu'à chaque niveau, la probabilité de basculement ou de redémarrage est plus importante.  Pour plus d'informations, consultez la section « Détermination des échecs » de la rubrique [Failover Policy for Failover Cluster Instances](../../../sql-server/failover-clusters/windows/failover-policy-for-failover-cluster-instances.md) .  
  
###  <a name="Security"></a> Sécurité  
  
####  <a name="Permissions"></a> Permissions  
 Nécessite les autorisations ALTER SETTINGS et VIEW SERVER STATE.  
  
##  <a name="PowerShellProcedure"></a> Utilisation de PowerShell  
  
##### <a name="to-configure-failureconditionlevel-settings"></a>Pour configurer les paramètres FailureConditionLevel  
  
1.  Démarrez Windows PowerShell avec élévation de privilèges via **Exécuter en tant qu'administrateur**.  
  
2.  Importez le module **FailoverClusters** pour activer les applets de commande de cluster.  
  
3.  Utilisez l’applet de commande **Get-ClusterResource** pour rechercher la ressource [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , puis utilisez l’applet de commande **Set-ClusterParameter** pour définir la propriété **FailureConditionLevel** pour une instance de cluster de basculement.  
  
> [!TIP]  
>  Chaque fois que vous ouvrez une nouvelle fenêtre PowerShell, vous devez importer le module **FailoverClusters** .  
  
### <a name="example-powershell"></a>Exemple (PowerShell)  
 L'exemple suivant modifie le paramètre FailureConditionLevel sur la ressource [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] «`SQL Server (INST1)`» par un basculement ou un redémarrage en cas d'erreurs de serveur critiques.  
  
```powershell  
Import-Module FailoverClusters  
  
$fci = "SQL Server (INST1)"  
Get-ClusterResource $fci | Set-ClusterParameter FailureConditionLevel 3  
  
```  
  
### <a name="related-content-powershell"></a>Contenu connexe (PowerShell)  
  
-   [Clustering and High-Availability](http://blogs.msdn.com/b/clustering/archive/2009/05/23/9636665.aspx) (Clustering et haute disponibilité) (Blog de l’équipe de clustering de basculement et d’équilibrage de la charge réseau)  
  
-   [Mise en route de Windows PowerShell sur un cluster de basculement](http://technet.microsoft.com/library/ee619762\(WS.10\).aspx)  
  
-   [Commandes de ressource de cluster et applets de commande Windows PowerShell équivalentes](http://msdn.microsoft.com/library/ee619744.aspx#BKMK_resource)  
  
##  <a name="WSFC"></a> Utilisation du composant logiciel enfichable Gestionnaire du cluster de basculement  
 **Pour configurer les paramètres de propriété FailureConditionLevel :**  
  
1.  Ouvrez le composant logiciel enfichable Gestionnaire du cluster de basculement.  
  
2.  Développez **Services et applications** et sélectionnez l'instance FCI.  
  
3.  Cliquez avec le bouton droit sur **Ressource SQL Server** sous **Autres ressources**puis, dans le menu, sélectionnez **Propriétés** . La boîte de dialogue **Propriétés** de la ressource SQL Server s'ouvre.  
  
4.  Sélectionnez l'onglet **Propriétés** , entrez la valeur souhaitée pour la propriété **FaliureConditionLevel** , puis cliquez sur **OK** pour appliquer la modification.  
  
##  <a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
 **Pour configurer les paramètres de propriété FailureConditionLevel :**  
  
 À l’aide de l’instruction [ALTER SERVER CONFIGURATION](../../../t-sql/statements/alter-server-configuration-transact-sql.md)[!INCLUDE[tsql](../../../includes/tsql-md.md)] , spécifiez la valeur de la propriété FailureConditionLevel.  
  
###  <a name="TsqlExample"></a> Exemple (Transact-SQL)  
 L'exemple suivant affecte à la propriété FailureConditionLevel la valeur 0, ce qui indique qu'aucun basculement ni redémarrage ne sera déclenché automatiquement sur n'importe quelle condition d'échec.  
  
```  
ALTER SERVER CONFIGURATION SET FAILOVER CLUSTER PROPERTY FailureConditionLevel = 0;  
```  
  
## <a name="see-also"></a> Voir aussi  
 [sp_server_diagnostics &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-server-diagnostics-transact-sql.md)   
 [Failover Policy for Failover Cluster Instances](../../../sql-server/failover-clusters/windows/failover-policy-for-failover-cluster-instances.md)  
  
  
