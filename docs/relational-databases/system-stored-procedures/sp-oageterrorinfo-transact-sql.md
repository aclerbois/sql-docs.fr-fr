---
title: sp_OAGetErrorInfo (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_OAGetErrorInfo_TSQL
- sp_OAGetErrorInfo
dev_langs:
- TSQL
helpviewer_keywords:
- sp_OAGetErrorInfo
ms.assetid: ceecea08-456f-4819-85d9-ecc9647d7187
caps.latest.revision: 16
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: b272cbcc6fc8221dede3b4a1274032926910186a
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spoageterrorinfo-transact-sql"></a>sp_OAGetErrorInfo (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Obtient des informations d'erreur OLE Automation.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_OAGetErrorInfo [ objecttoken ]  
    [ , source OUTPUT ]   
    [ , description OUTPUT ]   
    [ , helpfile OUTPUT ]   
    [ , helpid OUTPUT ]   
```  
  
## <a name="arguments"></a>Arguments  
 *objecttoken*  
 Est le jeton d’objet d’un objet OLE précédemment créé à l’aide de **sp_OACreate** ou sa valeur est NULL. Si *objecttoken* est spécifié, les informations d’erreur pour cet objet sont retournées. Si NULL est spécifié, les informations d'erreur sont renvoyées pour l'ensemble du traitement.  
  
 *source* **sortie**  
 Source des informations d'erreur. Si spécifié, il doit être une variable locale **char**, **nchar**, **varchar**, ou **nvarchar** variable. Si nécessaire, la valeur renvoyée est tronquée pour s'adapter à la variable locale.  
  
 *Description* **sortie**  
 Est la description de l’erreur. Si spécifié, il doit être une variable locale **char**, **nchar**, **varchar**, ou **nvarchar** variable. Si nécessaire, la valeur renvoyée est tronquée pour s'adapter à la variable locale.  
  
 *HelpFile* **sortie**  
 Fichier d'aide de l'objet OLE. Si spécifié, il doit être une variable locale **char**, **nchar**, **varchar**, ou **nvarchar** variable. Si nécessaire, la valeur renvoyée est tronquée pour s'adapter à la variable locale.  
  
 *HelpID* **sortie**  
 ID de contexte du fichier d'aide. Si spécifié, il doit être une variable locale **int** variable.  
  
> [!NOTE]  
>  Les paramètres pour cette procédure stockée sont spécifiés par position et non pas par nom.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (succès) ou un nombre différent de zéro (échec), qui représente la valeur entière de HRESULT renvoyée par l'objet OLE Automation.  
  
 Pour plus d’informations sur les Codes de retour HRESULT, consultez [OLE Automation Codes de retour et des informations d’erreur](../../relational-databases/stored-procedures/ole-automation-return-codes-and-error-information.md).  
  
## <a name="result-sets"></a>Jeux de résultats  
 Si aucun paramètre de sortie n'est spécifié, les informations d'erreur sont renvoyées au client sous la forme d'un ensemble de résultats.  
  
|Noms des colonnes|Type de données| Description|  
|------------------|---------------|-----------------|  
|**Erreur**|**binary (4)**|Représentation binaire du numéro d'erreur.|  
|**Source**|**nvarchar(nn)**|Source de l'erreur.|  
|**Description**|**nvarchar(nn)**|Description de l’erreur.|  
|**HelpFile**|**nvarchar(nn)**|Fichier d'aide pour la source.|  
|**HelpID**|**int**|ID du contexte de l'aide dans le fichier source d'aide.|  
  
## <a name="remarks"></a>Notes  
 Procédure stockée de chaque appel à une OLE Automation (à l’exception de **sp_OAGetErrorInfo**) réinitialise les informations d’erreur ; par conséquent, **sp_OAGetErrorInfo** Obtient des informations d’erreur uniquement pour l’appel de procédure stockée OLE Automation plus récente. Notez que puisque **sp_OAGetErrorInfo** ne réinitialise pas les informations d’erreur, il peut être appelée plusieurs fois pour obtenir les mêmes informations d’erreur.  
  
 Le tableau suivant donne la liste des erreurs OLE Automation et leurs causes courantes.  
  
|Erreur et HRESULT|Cause courante|  
|-----------------------|------------------|  
|**Mauvais type de variable (0 x 80020008)**|Type de données d’une [!INCLUDE[tsql](../../includes/tsql-md.md)] valeur passée comme un paramètre de méthode ne correspond pas à la [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] type de données du paramètre de méthode ou une valeur NULL a été passée comme un paramètre de méthode.|  
|**Nom inconnu (0 x 8002006)**|La propriété ou le nom de méthode spécifié n'a pu être trouvé pour l'objet spécifié.|  
|**Chaîne de classe non valide (0x800401f3)**|Le ProgID ou le CLSID spécifié n'est pas inscrit en tant qu'objet OLE sur une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Serveurs OLE automation personnalisés doivent être enregistrés avant de pouvoir être instanciés à l’aide de **sp_OACreate**. Cela est possible à l’aide de l’utilitaire Regsvr32.exe pour les serveurs intra-processus (.dll), ou le **/REGSERVER** commutateur de ligne de commande pour les serveurs locaux (.exe).|  
|**Échec de l’exécution du serveur (0 x 80080005)**|L'objet OLE spécifié est inscrit comme serveur OLE local (fichier .EXE), mais le fichier .EXE ne peut pas être trouvé ou démarré.|  
|**Le module spécifié est introuvable (0x8007007e)**|L'objet OLE spécifié est inscrit comme serveur OLE in-process (fichier .DLL), mais le fichier .DLL ne peut pas être trouvé ou chargé.|  
|**Incompatibilité de type (0 x 80020005)**|Le type de données d'une variable locale [!INCLUDE[tsql](../../includes/tsql-md.md)] utilisée pour stocker une valeur de propriété renvoyée ou une valeur de retour de méthode ne correspond pas au type de données [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] de la valeur de retour de la propriété ou de la méthode. La raison peut aussi être que la valeur de retour d'une propriété ou d'une méthode était requise, mais qu'aucune valeur n'a été renvoyée.|  
|**Le type de données ou la valeur du paramètre 'context' de sp_OACreate n'est pas valide. (0x8004275B)**|La valeur du paramètre de contexte doit être : 1, 4 ou 5.|  
  
 Pour plus d’informations sur le traitement des Codes de retour HRESULT, consultez [OLE Automation Codes de retour et des informations d’erreur](../../relational-databases/stored-procedures/ole-automation-return-codes-and-error-information.md).  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle serveur fixe **sysadmin** .  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant affiche des informations d'erreur OLE Automation.  
  
```  
DECLARE @output varchar(255);  
DECLARE @hr int;  
DECLARE @source varchar(255);  
DECLARE @description varchar(255);  
PRINT 'OLE Automation Error Information';  
EXEC @hr = sp_OAGetErrorInfo @object, @source OUT, @description OUT;  
IF @hr = 0  
BEGIN  
    SELECT @output = '  Source: ' + @source  
    PRINT @output  
    SELECT @output = '  Description: ' + @description  
    PRINT @output  
END  
ELSE  
BEGIN  
    PRINT '  sp_OAGetErrorInfo failed.'  
    RETURN  
END;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées OLE Automation &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/ole-automation-stored-procedures-transact-sql.md)   
 [Exemple de script OLE Automation](../../relational-databases/stored-procedures/ole-automation-sample-script.md)  
  
  
