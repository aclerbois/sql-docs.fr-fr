---
title: Configurer l’option de configuration de serveur remote query timeout | Microsoft Docs
ms.custom: ''
ms.date: 03/08/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.suite: sql
ms.technology: configuration
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- time limit for remote queries [SQL Server]
- remote query timeout option
ms.assetid: 888c8448-933b-41e3-8aa1-c206bc0cdb78
caps.latest.revision: 26
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 10c9e6e858f12e439b7fe0f95e3680ccf3246443
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="configure-the-remote-query-timeout-server-configuration-option"></a>Configurer l'option de configuration de serveur remote query timeout
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

  Cette rubrique explique comment configurer l'option de configuration de serveur **remote query timeout** dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)]. L'option **remote query timeout** spécifie la durée, en secondes, d'une opération distante au terme de laquelle le délai d'attente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] expire. La valeur par défaut de cette option est 600, qui correspond à une attente de 10 minutes. Cette valeur s'applique à une connexion sortante émise par le [!INCLUDE[ssDE](../../includes/ssde-md.md)] comme requête distante. Elle n'a aucun effet sur les requêtes reçues par le [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Pour désactiver le délai d'attente, affectez-lui la valeur 0. Une requête attend jusqu’à ce qu’elle se termine.  
  
 Pour les requêtes hétérogènes, l’option **remote query timeout** spécifie le nombre de secondes (initialisé dans l’objet commande à l’aide de la propriété d’ensemble de lignes DBPROP_COMMANDTIMEOUT) pendant lesquelles un fournisseur distant peut attendre les résultats avant l’expiration de la requête. Cette valeur est également utilisée pour définir DBPROP_GENERALTIMEOUT si elle est prise en charge par le fournisseur distant. Cela entraînera l'expiration du délai de toutes les autres opérations après le nombre de secondes spécifié.  
  
 Pour les procédures stockées distantes, l'option **remote query timeout** spécifie le nombre de secondes devant s'écouler après l'envoi d'une instruction distante `EXEC` avant que le délai d'attente ne soit atteint.  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Conditions préalables](#Prerequisites)  
  
     [Sécurité](#Security)  
  
-   **Pour configurer l'option remote query timeout, utilisez :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   **Suivi :**  [Après avoir configuré l'option remote query timeout](#FollowUp)  
  
##  <a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="Prerequisites"></a> Conditions préalables  
  
-   Les connexions au serveur distant doivent être autorisées avant que cette valeur puisse être définie.  
  
###  <a name="Security"></a> Sécurité  
  
####  <a name="Permissions"></a> Permissions  
 Les autorisations d’exécution de **sp_configure** , sans paramètre ou avec le premier paramètre uniquement, sont accordées par défaut à tous les utilisateurs. Pour exécuter **sp_configure** avec les deux paramètres afin de modifier une option de configuration ou d’exécuter l’instruction RECONFIGURE, un utilisateur doit disposer de l’autorisation de niveau serveur ALTER SETTINGS. L'autorisation ALTER SETTINGS est implicitement détenue par les rôles serveur fixes **sysadmin** et **serveradmin** .  
  
##  <a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-configure-the-remote-query-timeout-option"></a>Pour configurer l'option Délai d'attente de la requête distante  
  
1.  Dans l’Explorateur d’objets, cliquez avec le bouton droit sur un serveur et sélectionnez **Propriétés**.  
  
2.  Cliquez sur le nœud **Connexions** .  
  
3.  Sous **Connexions au serveur distant**, dans la zone **Délai d'attente de la requête distante** , tapez ou sélectionnez une valeur comprise entre 0 et 2 147 483 647 pour définir le nombre maximal de secondes de l'attente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] avant expiration.  
  
##  <a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-configure-the-remote-query-timeout-option"></a>Pour configurer l'option Délai d'attente de la requête distante  
  
1.  Connectez-vous au [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**. Cet exemple montre comment utiliser [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) pour attribuer à l’option `remote query timeout` la valeur `0` afin de désactiver le délai d’attente.  
  
```sql  
USE AdventureWorks2012 ;  
GO  
EXEC sp_configure 'remote query timeout', 0 ;  
GO  
RECONFIGURE ;  
GO  
  
```  
  
 Pour plus d’informations, consultez [Options de configuration de serveur &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md).  
  
##  <a name="FollowUp"></a> Suivi : Après avoir configuré l'option Délai d'attente de la requête distante  
 Le paramètre prend effet immédiatement sans redémarrage du serveur.  
  
## <a name="see-also"></a> Voir aussi  
 [RECONFIGURE &#40;Transact-SQL&#41;](../../t-sql/language-elements/reconfigure-transact-sql.md)   
 [Propriétés et comportements de l’ensemble de lignes](../../relational-databases/native-client-ole-db-rowsets/rowset-properties-and-behaviors.md)   
 [Options de configuration de serveur &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)  
  
  
