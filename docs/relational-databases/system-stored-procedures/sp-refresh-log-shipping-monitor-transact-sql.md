---
title: sp_refresh_log_shipping_monitor (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_refresh_log_shipping_monitor
- sp_refresh_log_shipping_monitor_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_refresh_log_shipping_monitor
ms.assetid: edefb912-31c5-4d99-9aba-06629afd0171
caps.latest.revision: 21
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: f2f0bf6df228f7a0948b0c5d295ae6d2cdf4a9b9
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="sprefreshlogshippingmonitor-transact-sql"></a>sp_refresh_log_shipping_monitor (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Cette procédure stockée actualise les tables de moniteurs distants avec les dernières informations provenant d'un serveur principal ou secondaire spécifique pour l'Agent de copie des journaux de transaction. La procédure est appelée sur le serveur principal ou secondaire.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_refresh_log_shipping_monitor  
[ @agent_id = ] 'agent_id',  
[ @agent_type = ] 'agent_type'  
[ @database = ] 'database'  
[ @mode ] n  
```  
  
## <a name="arguments"></a>Arguments  
 [  **@agent_id=** ] **'***agent_id***'**  
 ID principal pour la sauvegarde ou ID secondaire pour la copie ou la restauration. *éléments agent_id* est **uniqueidentifier** et ne peut pas être NULL.  
  
 [  **@agent_type=** ] **'***agent_type***'**  
 Type d'opération de copie des journaux de transaction.  
  
 0 = Sauvegarde.  
  
 1 = Copie.  
  
 2 = Restauration.  
  
 *agent_type* est **tinyint** et ne peut pas être NULL.  
  
 [  **@database=** ] **'***base de données***'**  
 Base de données primaire ou secondaire utilisée pour la connexion par des agents de sauvegarde ou de restauration.  
  
 [ **@mode** ] *n*  
 Spécifie s'il faut actualiser les données du moniteur ou les effacer. Type de données de *m* est de type tinyint et que les valeurs prises en charge sont :  
  
 1 = actualisation (Il s'agit de la valeur par défaut.)  
  
 2 = delete  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Aucun.  
  
## <a name="remarks"></a>Notes  
 **sp_refresh_log_shipping_monitor** actualise le **log_shipping_monitor_primary**, **log_shipping_monitor_secondary**, **log_shipping_monitor_history_detail** , et **log_shipping_monitor_error_detail** tables avec toutes les informations de session qui n’a pas déjà été transféré. Vous pouvez ainsi synchroniser le serveur moniteur avec un serveur principal ou secondaire lorsque sa dernière synchronisation remonte à un certain temps et vous pouvez si nécessaire y nettoyer les informations de moniteur.  
  
 **sp_refresh_log_shipping_monitor** doit être exécuté à partir de la **master** base de données sur le serveur principal ou secondaire.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres de la **sysadmin** rôle serveur fixe peut exécuter cette procédure.  
  
## <a name="see-also"></a>Voir aussi  
 [À propos de journaux de transaction & #40 ; SQL Server & #41 ;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
