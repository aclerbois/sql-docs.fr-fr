---
title: Sys.sp_rda_test_connection (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology:
- dbe-stretch
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sys.sp_rda_test_connection
- sys.sp_rda_test_connection_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sp_rda_test_connection stored procedure
ms.assetid: e2ba050c-d7e3-4f33-8281-c9b525b4edb4
caps.latest.revision: 7
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 1c44c6c3974b10b442d4b6a5944276f7f3c3693e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="syssprdatestconnection-transact-sql"></a>sys.sp_rda_test_connection (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2016-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2016-xxxx-xxxx-xxx-md.md)]

  Teste la connexion à partir de SQL Server pour le serveur Azure distant et signale des problèmes qui peuvent empêcher la migration des données.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
EXECUTE sys.sp_rda_test_connection  
   @database_name = N'db_name',   
   @server_address = N'azure_server_fully_qualified_address',  
   @azure_username = N'azure_username',   
   @azure_password = N'azure_password',  
   @credential_name = N'credential_name'  
  
```  
  
## <a name="arguments"></a>Arguments  
 @database_name = N'*db_name*'  
 Le nom de la base de données SQL Server compatible Stretch. Ce paramètre est facultatif.  
  
 @server_address = N'*azure_server_fully_qualified_address*'  
 L’adresse complète du serveur Azure.  
  
-   Si vous fournissez une valeur pour **@database_name**, mais la base de données spécifié n’est pas compatible avec Stretch, alors vous devez fournir une valeur pour **@server_address**.  
  
-   Si vous fournissez une valeur pour **@database_name**et la base de données spécifiée est compatible avec Stretch, alors que vous n’êtes pas obligé de fournir une valeur pour **@server_address**. Si vous fournissez une valeur pour **@server_address**, la procédure stockée ignore et utilise les Azure serveur déjà associé à la base de données Stretch.  
  
 @azure_username = N'*azure_username*  
 Le nom d’utilisateur pour le serveur Azure distant.  
  
 @azure_password = N'*azure_password*'  
 Le mot de passe pour le serveur Azure distant.  
  
 @credential_name = N'*credential_name*'  
 Au lieu de fournir un nom d’utilisateur et un mot de passe, vous pouvez fournir le nom d’informations d’identification stockée dans la base de données Stretch.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 Dans le cas de **réussite**, sp_rda_test_connection renvoie une erreur 14855 (STRETCH_MAJOR, STRETCH_CONNECTION_TEST_PROC_SUCCEEDED) avec la gravité EX_INFO et code de retour de succès.  
  
 Dans le cas de **échec**, sp_rda_test_connection renvoie une erreur 14856 (STRETCH_MAJOR, STRETCH_CONNECTION_TEST_PROC_FAILED) avec la gravité EX_USER et code de retour une erreur.  
  
## <a name="result-sets"></a>Jeux de résultats  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|LINK_STATE accompagnées|int|Une des valeurs suivantes, qui correspondent aux valeurs de **link_state_desc**.<br /><br /> -   0<br />-   1<br />-   2<br />-   3<br />-   4|  
|link_state_desc|varchar(32)|Une des valeurs suivantes, qui correspondent à l’exemple précédent valeurs pour **LINK_STATE accompagnées**.<br /><br /> -SAIN<br />     Le serveur entre SQL Server et Azure à distance n'est sain.<br />-ERROR_AZURE_FIREWALL<br />     Le pare-feu Azure empêche le lien entre SQL Server et le serveur Azure distant.<br />-ERROR_NO_CONNECTION<br />     SQL Server ne peut pas établir une connexion au serveur Azure distant.<br />-ERROR_AUTH_FAILURE<br />     Échec de l’authentification empêche le lien entre SQL Server et le serveur Azure distant.<br />-ERREUR<br />     Une erreur qui n’est pas un problème d’authentification, un problème de connectivité ou d’un problème de pare-feu empêche le lien entre SQL Server et le serveur Azure distant.|  
|error_number|int|Le numéro de l’erreur. S’il n’existe aucune erreur, ce champ est NULL.|  
|error_message|nvarchar(1024)|Message d’erreur. S’il n’existe aucune erreur, ce champ est NULL.|  
  
## <a name="permissions"></a>Autorisations  
 Nécessite les autorisations db_owner.  
  
## <a name="examples"></a>Exemples  
  
### <a name="check-the-connection-from-sql-server-to-the-remote-azure-server"></a>Vérifiez la connexion à partir de SQL Server pour le serveur Azure distant  
  
```sql  
EXECUTE sys.sp_rda_test_connection @database_name = N'<Stretch-enabled database>'  
GO  
  
```  
  
 Les résultats indiquent que SQL Server ne peut pas se connecter au serveur Azure distant.  
  
|LINK_STATE accompagnées|link_state_desc|error_number|error_message|  
|-----------------|-----------------------|-------------------|--------------------|  
|2|ERROR_NO_CONNECTION|*\<numéro d’erreur de connexion >*|*\<message d’erreur relatif à la connexion >*|  
  
### <a name="check-the-azure-firewall"></a>Vérifiez le pare-feu Azure  
  
```sql  
USE <Stretch-enabled database>  
GO  
EXECUTE sys.sp_rda_test_connection  
GO  
  
```  
  
 Les résultats indiquent que le pare-feu Azure n’empêche pas le lien entre SQL Server et le serveur Azure distant.  
  
|LINK_STATE accompagnées|link_state_desc|error_number|error_message|  
|-----------------|-----------------------|-------------------|--------------------|  
|1|ERROR_AZURE_FIREWALL|*\<numéro d’erreur liés au pare-feu >*|*\<message d’erreur liés au pare-feu >*|  
  
### <a name="check-authentication-credentials"></a>Vérifiez les informations d’identification d’authentification  
  
```sql  
USE <Stretch-enabled database>  
GO  
EXECUTE sys.sp_rda_test_connection  
GO  
  
```  
  
 Les résultats indiquent qu’un échec d’authentification n’empêche pas le lien entre SQL Server et le serveur Azure distant.  
  
|LINK_STATE accompagnées|link_state_desc|error_number|error_message|  
|-----------------|-----------------------|-------------------|--------------------|  
|3|ERROR_AUTH_FAILURE|*\<numéro d’erreur de liés à l’authentification >*|*\<message d’erreur d’authentification >*|  
  
### <a name="check-the-status-of-the-remote-azure-server"></a>Vérifier l’état du serveur Azure distant  
  
```sql  
USE <SQL Server database>  
GO  
EXECUTE sys.sp_rda_test_connection   
    @server_address = N'<server name>.database.windows.net',   
    @azure_username = N'<user name>',   
    @azure_password = N'<password>'  
GO  
  
```  
  
 Les résultats indiquent que la connexion est intègre et que vous pouvez activer Stretch Database pour la base de données spécifié.  
  
|LINK_STATE accompagnées|link_state_desc|error_number|error_message|  
|-----------------|-----------------------|-------------------|--------------------|  
|0|HEALTHY|NULL|NULL|  
  
  
