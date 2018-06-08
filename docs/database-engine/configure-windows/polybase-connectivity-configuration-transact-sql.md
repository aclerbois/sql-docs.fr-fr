---
title: Configuration de la connectivité PolyBase (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/03/2017
ms.prod: sql
ms.prod_service: database-engine, pdw
ms.reviewer: ''
ms.suite: sql
ms.technology: configuration
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- PolyBase
ms.assetid: 82252e4f-b1d0-49e5-aa0b-3624aade2add
caps.latest.revision: 14
author: MashaMSFT
ms.author: mathoma
manager: craigg
monikerRange: '>= aps-pdw-2016 || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: bb82ed8c4a4dc7c881ac2b94dee9ea88ce009858
ms.sourcegitcommit: d2573a8dec2d4102ce8882ee232cdba080d39628
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/07/2018
---
# <a name="polybase-connectivity-configuration-transact-sql"></a>Configuration de la connectivité PolyBase (Transact-SQL)
[!INCLUDE[appliesto-ss-xxxx-xxxx-pdw-md](../../includes/appliesto-ss-xxxx-xxxx-pdw-md.md)]

  Affiche ou modifie les paramètres de configuration globale pour la connectivité du stockage d'objets blob Azure et PolyBase Hadoop.
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
--List all of the configuration options  
sp_configure  
[;]  
  
--Configure Hadoop connectivity  
sp_configure [ @configname = ] 'hadoop connectivity',  
             [ @configvalue = ] { 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 }  
[;]  
  
RECONFIGURE  
[;]  
```  
  
## <a name="arguments"></a>Arguments  
 [ **@configname=** ] **'***option_name***'**  
 Nom d'une option de configuration. *option_name* est **varchar(35)**, avec NULL comme valeur par défaut. Si ce dernier n'est pas spécifié, la liste complète des options est renvoyée.  
  
 [ **@configvalue=** ] **'***value***'**  
 Nouveau paramètre de configuration. *value* est de type **int**, avec NULL comme valeur par défaut. La valeur maximale dépend de l'option individuelle.  
  
 **'connectivité hadoop'**  
 Spécifie le type de source de données Hadoop pour toutes les connexions à partir de PolyBase pour des clusters Hadoop ou un stockage d'objets blob Azure (WASB). Ce paramètre est nécessaire à la création d’une source de données externe pour une table externe. Pour plus d’informations, consultez la rubrique [CRÉER UNE SOURCE DE DONNÉES EXTERNE (Transact-SQL)](../../t-sql/statements/create-external-data-source-transact-sql.md),  
  
 Voici les paramètres de connectivité Hadoop et leurs sources de données Hadoop prises en charge correspondantes. Un seul paramètre peut être activé à la fois. Les options 1, 4 et 7 permettent de créer plusieurs types de sources de données externes et de les utiliser dans toutes les sessions sur le serveur.  
  
-   Option 0 : désactiver la connectivité Hadoop  
  
-   Option 1 : Hortonworks HDP 1.3 sur Windows Server  
  
-   Option 1 : stockage d’objets blob Azure (WASB[S])  
  
-   Option 2 : Hortonworks HDP 1.3 sur Linux  
  
-   Option 3 : Cloudera CDH 4.3 sur Linux  
  
-   Option 4 : Hortonworks HDP 2.0 sur Windows Server  
  
-   Option 4 : stockage d’objets blob Azure (WASB[S])  
  
-   Option 5 : Hortonworks HDP 2.0 sur Linux  
  
-   Option 6 : Cloudera 5.1, 5.2, 5.3, 5.4, 5.5, 5.9, 5.10, 5.11, 5.12 et 5.13 sur Linux  
  
-   Option 7 : Hortonworks 2.1, 2.2, 2.3, 2.4, 2.5 et 2.6 sur Linux  
  
-   Option 7 : Hortonworks 2.1, 2.2 et 2.3 sur Windows Server  
  
-   Option 7 : stockage d’objets blob Azure (WASB[S])  
  
 **RECONFIGURE**  
 Met à jour la valeur d'exécution (run_value) afin qu’elle corresponde à la valeur de configuration (config_value). Pour connaître les définitions des valeurs run_value et config_value, consultez [Jeux de résultats](#ResultSets) . La nouvelle valeur de configuration définie par sp_configure n’est pas effective tant que la valeur d'exécution n’a pas été définie par l'instruction RECONFIGURE.  
  
 Après l'exécution de RECONFIGURE, vous devez arrêter et redémarrer le service SQL Server. Notez que lors de l'arrêt du service SQL Server, les deux services supplémentaires du moteur PolyBase et du déplacement des données s’arrêteront automatiquement. Après le redémarrage du service du moteur SQL Server, redémarrez les deux services (s’ils ne démarrent pas automatiquement).  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (réussite) ou 1 (échec)  
  
##  <a name="ResultSets"></a> Jeux de résultats  
 Lorsqu’elle est exécutée sans paramètres, la valeur **sp_configure** renvoie un jeu de résultats à cinq colonnes.  
  
|Nom de colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**nom**|**nvarchar(35)**|Nom de l'option de configuration.|  
|**minimum**|**Int**|Valeur minimale de l'option de configuration.|  
|**maximum**|**Int**|Valeur maximale de l'option de configuration.|  
|**config_value**|**Int**|Valeur qui avait été définie à l’aide de **sp_configure**.|  
|**run_value**|**Int**|Valeur actuelle en cours d'utilisation par PolyBase. Cette valeur est définie en exécutant RECONFIGURE.<br /><br /> Les valeurs **config_value** et **run_value** sont généralement identiques, sauf si la valeur est en cours de modification.<br /><br /> Un redémarrage peut être nécessaire pour obtenir une valeur d'exécution précise, si la reconfiguration est en cours.|  
  
## <a name="general-remarks"></a>Remarques d'ordre général  
 Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], après exécution de RECONFIGURE, vous devez redémarrer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]pour activer la connectivité hadoop.  
Dans [!INCLUDE[ssPDW](../../includes/sspdw-md.md)], après exécution de RECONFIGURE, vous devez redémarrer la région [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] pour activer la connectivité hadoop.  
  
## <a name="limitations-and-restrictions"></a>Limitations et restrictions  
 RECONFIGURE n'est pas autorisée dans une transaction explicite ou implicite.  
  
## <a name="permissions"></a>Autorisations  
 Tous les utilisateurs peuvent exécuter **sp_configure** sans paramètres ou avec le paramètre @configname .  
  
 Nécessite une autorisation **ALTER SETTINGS** au niveau du serveur ou l’appartenance rôle de serveur **sysadmin** pour modifier une valeur de configuration ou pour exécuter RECONFIGURE.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-list-all-available-configuration-settings"></a>A. Liste de tous les paramètres de configuration disponibles  
 L'exemple suivant montre comment afficher toutes les options de configuration.  
  
```  
EXEC sp_configure;  
```  
  
 Le résultat renvoie le nom de l'option suivi des valeurs minimales et maximales de cette option. **config_value** est la valeur que SQL ou PolyBase utilisera une fois la reconfiguration terminée. **run_value** est la valeur en cours d’utilisation. Les valeurs **config_value** et **run_value** sont généralement identiques, sauf si la valeur est en cours de modification.  
  
### <a name="b-list-the-configuration-settings-for-one-configuration-name"></a>B. Liste des paramètres de configuration pour un nom de configuration  
  
```  
EXEC sp_configure @configname='hadoop connectivity';  
```  
  
### <a name="c-set-hadoop-connectivity"></a>C. Définir la connectivité Hadoop  
 Cet exemple définit PolyBase sur l’option 7. Cette option permet à PolyBase de créer et d’utiliser des tables externes sur Hortonworks 2.1, 2.2 et 2.3 sous Linux et Windows Server, et le stockage d'objets blob Azure. Par exemple, SQL peut avoir 30 tables externes dont 7 d’entre-elles référencent des données sur Hortonworks 2.1 sous Linux, 4 sur Hortonworks 2.2 sous Linux, 7 sur Hortonworks 2.3 sous Linux, et les 12 autres référençant le stockage d'objets blob Azure.  
  
```  
--Configure external tables to reference data on Hortonworks 2.1, 2.2, and 2.3 on Linux, and Azure blob storage  
  
sp_configure @configname = 'hadoop connectivity', @configvalue = 7;  
GO  
  
RECONFIGURE  
GO  
```  
  
## <a name="see-also"></a> Voir aussi  
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)   
 [CREATE EXTERNAL TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-table-transact-sql.md)   
 [CREATE EXTERNAL FILE FORMAT &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-file-format-transact-sql.md)   
 [CREATE EXTERNAL DATA SOURCE &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-data-source-transact-sql.md)  
  
  
