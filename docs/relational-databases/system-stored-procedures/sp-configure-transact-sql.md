---
title: sp_configure (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/16/2016
ms.prod: sql
ms.prod_service: database-engine, pdw
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_configure
- sp_configure_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_configure
ms.assetid: d18b251d-b37a-4f5f-b50c-502d689594c8
caps.latest.revision: 60
author: edmacauley
ms.author: edmaca
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-mi-current || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 05074a051f39e8b2dd81314ed6e230868c410c49
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spconfigure-transact-sql"></a>sp_configure (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-pdw-md](../../includes/t-sql-appliesto-ss-asdbmi-xxxx-pwd-md.md)]

  Affiche ou modifie des options de configuration générales pour le serveur actif.

[!INCLUDE[ssMIlimitation](../../includes/sql-db-mi-limitation.md)]
  
> [!NOTE]  
>  Pour plus d’options de configuration au niveau de la base de données, consultez [ALTER DATABASE SCOPED CONFIGURATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md). Pour configurer Soft-NUMA, consultez [Soft-NUMA &#40;SQL Server&#41;](../../database-engine/configure-windows/soft-numa-sql-server.md).  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
-- Syntax for SQL Server  
  
sp_configure [ [ @configname = ] 'option_name'   
    [ , [ @configvalue = ] 'value' ] ]  
```  
  
```  
-- Syntax for Parallel Data Warehouse  
  
-- List all of the configuration options  
sp_configure  
[;]  
  
-- Configure Hadoop connectivity  
sp_configure [ @configname= ] 'hadoop connectivity',  
             [ @configvalue = ] { 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 }  
[;]  
RECONFIGURE  
[;]  
```  
  
## <a name="arguments"></a>Arguments  
 [ **@configname=** ] **'***option_name***'**  
 Nom d'une option de configuration. *option_name* est **varchar(35)**, avec NULL comme valeur par défaut. [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] reconnaît toute chaîne unique qui fait partie du nom de configuration. Si ce dernier n'est pas spécifié, la liste complète des options est renvoyée.  
  
 Pour plus d’informations sur les options de configuration disponibles et leurs paramètres, consultez [les Options de Configuration de serveur &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md).  
  
 [ **@configvalue=** ] **'***value***'**  
 Nouveau paramètre de configuration. *value* est de type **int**, avec NULL comme valeur par défaut. La valeur maximale dépend de l'option individuelle.  
  
 Pour obtenir la valeur maximale de chaque option, consultez la **maximale** colonne de la **sys.configurations** affichage catalogue.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="result-sets"></a>Jeux de résultats  
 Lorsqu’elle est exécutée sans paramètres, **sp_configure** renvoie un jeu de résultats avec cinq colonnes et trie les options par ordre alphabétique en ordre croissant, comme indiqué dans le tableau suivant.  
  
 Les valeurs de **config_value** et **run_value** ne sont pas automatiquement équivalentes. Après la mise à jour d’un paramètre de configuration à l’aide de **sp_configure**, l’administrateur système doit mettre à jour la valeur de configuration en cours d’exécution à l’aide de RECONFIGURE ou RECONFIGURE WITH OVERRIDE. Pour plus d'informations, consultez la section Notes.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**nom**|**nvarchar(35)**|Nom de l'option de configuration.|  
|**minimum**|**int**|Valeur minimale de l'option de configuration.|  
|**maximum**|**int**|Valeur maximale de l'option de configuration.|  
|**config_value**|**int**|Valeur à laquelle l’option de configuration a été définie à l’aide de **sp_configure** (valeur dans **sysconfigures.Value**). Pour plus d’informations sur ces options, consultez [les Options de Configuration de serveur &#40;SQL Server&#41; ](../../database-engine/configure-windows/server-configuration-options-sql-server.md) et [sys.configurations &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-configurations-transact-sql.md).|  
|**run_value**|**int**|Valeur de l’option de configuration en cours d’exécution (valeur dans **sys.configurations.value_in_use**).<br /><br /> Pour plus d’informations, consultez [sys.configurations &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-configurations-transact-sql.md).|  
  
## <a name="remarks"></a>Notes  
 Utilisez **sp_configure** pour afficher ou modifier les paramètres au niveau du serveur. Pour modifier les paramètres au niveau de la base de données, utilisez ALTER DATABASE. Pour modifier uniquement les paramètres qui ont une incidence sur la session de l'utilisateur actuel, utilisez l'instruction SET.  
  
## <a name="updating-the-running-configuration-value"></a>Mise à jour de la valeur de configuration en cours d'exécution  
 Lorsque vous spécifiez un nouveau *valeur* pour un *option*, le jeu de résultats affiche cette valeur dans la **config_value** colonne. Cette valeur est initialement diffère de la valeur dans la **run_value** colonne, qui affiche la valeur de configuration en cours d’exécution. Pour mettre à jour la valeur de configuration en cours d’exécution dans le **run_value** colonne, l’administrateur système doit exécuter RECONFIGURE ou RECONFIGURE WITH OVERRIDE.  
  
 Ces deux instructions fonctionnent avec toutes les options de configuration. Cependant, l'instruction de base RECONFIGURE rejette toute valeur d'option se trouvant en dehors d'une plage appropriée ou pouvant être à l'origine de conflits parmi les options. Par exemple, RECONFIGURE génère une erreur si le **l’intervalle de récupération** valeur est supérieure à 60 minutes ou si le **masque d’affinité** valeur chevauche le **masque d’affinité d’e/s** valeur. Par contre, RECONFIGURE WITH OVERRIDE accepte toutes les valeurs d'option présentant le bon type de données et force la reconfiguration avec la valeur spécifiée.  
  
> [!CAUTION]  
>  Une valeur d'option inappropriée peut avoir des répercussions négatives sur la configuration de l'instance de serveur. Utilisez RECONFIGURE WITH OVERRIDE avec prudence.  
  
 Certaines options peuvent être mises à jour de façon dynamique par l'instruction RECONFIGURE, alors que d'autres nécessitent un arrêt et un redémarrage du serveur. Par exemple, le **options min server memory** et **mémoire maximum du serveur** options mémoire du serveur sont mises à jour dynamiquement le [!INCLUDE[ssDE](../../includes/ssde-md.md)]; par conséquent, vous pouvez les modifier sans redémarrer le serveur. En revanche, reconfigurer la valeur en cours d’exécution de la **facteur de remplissage** option nécessite le redémarrage de le [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
 Après l’exécution de RECONFIGURE sur une option de configuration, vous pouvez voir si l’option a été mis à jour dynamiquement en exécutant **sp_configure'***option_name***'**. Les valeurs dans le **run_value** et **config_value** colonnes doivent correspondre pour une option de mise à jour dynamiquement. Vous pouvez également vérifier quelles sont les options dynamiques en examinant le **is_dynamic** colonne de la **sys.configurations** affichage catalogue.  
  
> [!NOTE]  
>  Si un *valeur* est trop élevée pour une option, le **run_value** colonne reflète le fait que la [!INCLUDE[ssDE](../../includes/ssde-md.md)] a pas réussi à la mémoire dynamique au lieu d’utiliser un paramètre qui n’est pas valide.  
  
 Pour plus d’informations, consultez [reconfigurer &#40;Transact-SQL&#41;](../../t-sql/language-elements/reconfigure-transact-sql.md).  
  
## <a name="advanced-options"></a>Options avancées  
 Certaines options de configuration, tel que **masque d’affinité** et **l’intervalle de récupération**, sont désignés comme des options avancées. Par défaut, ces options ne sont ni affichables ni modifiables. Pour les rendre disponibles, vous devez définir le **ShowAdvancedOptions** option de configuration à 1.  
  
 Pour plus d’informations sur les options de configuration et leurs paramètres, consultez [les Options de Configuration de serveur &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md).  
  
## <a name="permissions"></a>Autorisations  
 Les autorisations d’exécution de **sp_configure** , sans paramètre ou avec le premier paramètre uniquement, sont accordées par défaut à tous les utilisateurs. Pour exécuter **sp_configure** avec les deux paramètres pour modifier une option de configuration ou d’exécuter l’instruction RECONFIGURE, vous devez avoir l’autorisation ALTER SETTINGS au niveau du serveur. L'autorisation ALTER SETTINGS est implicitement détenue par les rôles serveur fixes **sysadmin** et **serveradmin** .  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-listing-the-advanced-configuration-options"></a>A. Affichage des options de configuration avancées  
 L'exemple suivant montre comment définir et afficher toutes les options de configuration. Pour afficher les options de configuration avancées, il faut tout d'abord donner à l'argument `show advanced option` la valeur `1`. Une fois cette option modifiée, l'exécution de `sp_configure` sans paramètre renvoie l'ensemble des options de configuration.  
  
```  
USE master;  
GO  
EXEC sp_configure 'show advanced option', '1';  
```  
  
 Voici le message : « L'option de configuration 'show advanced options' est passée de 0 à 1. Pour installer, exécutez l'instruction RECONFIGURE. »  
  
 Exécutez `RECONFIGURE` et affichez toutes les options de configuration :  
  
```  
RECONFIGURE;  
EXEC sp_configure;  
```  
  
### <a name="b-changing-a-configuration-option"></a>B. Modification d'une option de configuration  
 L'exemple suivant définit la valeur de l'option système `recovery interval` à `3` minutes.  
  
```  
USE master;  
GO  
EXEC sp_configure 'recovery interval', '3';  
RECONFIGURE WITH OVERRIDE;  
```  
  
## <a name="examples-includesspdwincludessspdw-mdmd"></a>Exemples : [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-list-all-available-configuration-settings"></a>C. Liste de tous les paramètres de configuration disponibles  
 L'exemple suivant montre comment afficher toutes les options de configuration.  
  
```  
EXEC sp_configure;  
```  
  
 Le résultat renvoie le nom de l'option suivi des valeurs minimales et maximales de cette option. Le **config_value** est la valeur qui [!INCLUDE[ssDW](../../includes/ssdw-md.md)] utilisera lors de la reconfiguration terminée. **run_value** est la valeur en cours d’utilisation. Les valeurs **config_value** et **run_value** sont généralement identiques, sauf si la valeur est en cours de modification.  
  
### <a name="d-list-the-configuration-settings-for-one-configuration-name"></a>D. Liste des paramètres de configuration pour un nom de configuration  
  
```  
EXEC sp_configure @configname='hadoop connectivity';  
```  
  
### <a name="e-set-hadoop-connectivity"></a>E. Définir la connectivité Hadoop  
 Définition de la connectivité Hadoop requiert quelques étapes supplémentaires en plus des sp_configure. Pour connaître la procédure complète, consultez [CREATE EXTERNAL DATA SOURCE &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-data-source-transact-sql.md).  
  
## <a name="see-also"></a>Voir aussi  
 [RECONFIGURE & #40 ; Transact-SQL & #41 ;](../../t-sql/language-elements/reconfigure-transact-sql.md)   
 [Instructions SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)   
 [Options de configuration de serveur &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [sys.configurations &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-configurations-transact-sql.md)   
 [ALTER DATABASE SCOPED CONFIGURATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md)   
 [Soft-NUMA &#40;SQL Server&#41;](../../database-engine/configure-windows/soft-numa-sql-server.md)  
  
  
