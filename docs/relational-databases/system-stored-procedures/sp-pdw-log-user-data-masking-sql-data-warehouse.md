---
title: sp_pdw_log_user_data_masking (SQL Data Warehouse) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: ''
ms.prod_service: sql-data-warehouse, pdw
ms.service: sql-data-warehouse
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: data-warehouse
ms.tgt_pltfrm: ''
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: 43c63b42-03cb-4fb5-8362-ec3b7e22a590
author: ronortloff
ms.author: rortloff
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest || = sqlallproducts-allversions'
ms.openlocfilehash: 6b158d6c0ea65159d6e310512ee45abb4916a0e7
ms.sourcegitcommit: d2573a8dec2d4102ce8882ee232cdba080d39628
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/07/2018
---
# <a name="sppdwloguserdatamasking-sql-data-warehouse"></a>sp_pdw_log_user_data_masking (entrepôt de données SQL)
[!INCLUDE[tsql-appliesto-xxxxxx-xxxx-asdw-pdw-md](../../includes/tsql-appliesto-xxxxxx-xxxx-asdw-pdw-md.md)]

  Utilisez **sp_pdw_log_user_data_masking** afin d’autoriser les données utilisateur de masquage dans [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] journaux d’activité. Le masquage des données utilisateur affecte les instructions de toutes les bases de données sur le matériel.  
  
> [!IMPORTANT]  
>  Le [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] journaux d’activité faisant l’objet **sp_pdw_log_user_data_masking** certaines [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] journaux d’activité. **sp_pdw_log_user_data_masking** n’affecte pas les journaux des transactions de base de données, ou [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] les journaux d’erreurs.  
  
 **Arrière-plan :** dans la configuration par défaut [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] contiennent des journaux d’activité complète [!INCLUDE[tsql](../../includes/tsql-md.md)] instructions et peut parfois incluent les données utilisateur contenues dans les opérations telles que **insérer**,  **Mise à jour**, et **sélectionnez** instructions. En cas de problème sur le matériel, cela permet à l’analyse des conditions qui a provoqué le problème sans avoir à reproduire le problème. Pour empêcher que les données utilisateur sont écrites dans [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] journaux d’activité, les clients peuvent choisir d’activer sur le masquage des données utilisateur à l’aide de cette procédure stockée. Les instructions seront toujours écrits dans [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] journaux d’activité, mais tous les littéraux dans les instructions qui contiennent des données utilisateur seront masqués ; remplacées par des valeurs de constante prédéfinies.  
  
 Lorsque le chiffrement transparent des données est activé sur le matériel, le masquage des données utilisateur dans [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] journaux d’activité est automatiquement activé.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
-- Syntax for Azure SQL Data Warehouse and Parallel Data Warehouse  
  
sp_pdw_log_user_data_masking [ [ @masking_mode = ] value ] ;  
```  
  
#### <a name="parameters"></a>Paramètres  
 [  **@masking_mode=** ] *masking_mode*  
 Détermine si les données d’utilisateur du journal de chiffrement transparent des données de masquage sont activées. *masking_mode* est **int**, et peut prendre l’une des valeurs suivantes :  
  
-   0 = désactivé, l’utilisateur, les données apparaissent dans le [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] journaux d’activité.  
  
-   1 = activé, l’utilisateur d’instructions de données s’affichent dans le [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] journaux d’activité, mais les données utilisateur est masquée.  
  
-   2 = les instructions contenant des données utilisateur ne sont pas écrites dans le [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] journaux d’activité.  
  
 L’exécution de **sp_pdw_ log_user_data_masking** sans paramètres retourne l’état actuel de masquage des données de chiffrement transparent des données journal utilisateur sur l’application en tant qu’un jeu de résultats scalaires.  
  
## <a name="remarks"></a>Notes  
 Masquage de données de l’utilisateur [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] permettre le remplacement de littéraux des journaux d’activité avec les valeurs de constante prédéfinies dans **sélectionnez** et des instructions DML, car elles peuvent contenir des données utilisateur. Paramètre *masking_mode* 1 ne masque pas de métadonnées, telles que les noms de colonne ou table. Paramètre *masking_mode* 2 supprime les instructions avec des métadonnées, telles que les noms de colonne ou table.  
  
 Les données utilisateur de masquage dans [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] journaux d’activité est implémenté de la manière suivante :  
  
-   Chiffrement transparent des données et les données utilisateur de masquage dans [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] journaux d’activité sont désactivés par défaut. Les instructions ne seront pas automatiquement masquées si le chiffrement de base de données n’est pas activé sur l’appareil.  
  
-   Activation de TDE automatiquement sur l’application active sur le masquage des données utilisateur dans [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] journaux d’activité.  
  
-   La désactivation du chiffrement transparent des données n’affecte pas les données utilisateur de masquage dans [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] journaux d’activité.  
  
-   Vous pouvez activer explicitement de masquage dans des données utilisateur [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] journaux d’activité à l’aide de la **sp_pdw_log_user_data_masking** procédure.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l’appartenance dans le **sysadmin** rôle de base de données fixe ou **CONTROL SERVER** autorisation.  
  
## <a name="example"></a>Exemple  
 L’exemple suivant active le chiffrement transparent des données journal utilisateur masquage des données sur le matériel.  
  
```  
EXEC sp_pdw_log_user_data_masking 1;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [sp_pdw_database_encryption pour &#40;SQL Data Warehouse&#41;](../../relational-databases/system-stored-procedures/sp-pdw-database-encryption-sql-data-warehouse.md)   
 [sp_pdw_database_encryption_regenerate_system_keys &#40;SQL Data Warehouse&#41;](../../relational-databases/system-stored-procedures/sp-pdw-database-encryption-regenerate-system-keys-sql-data-warehouse.md)  
  
  
