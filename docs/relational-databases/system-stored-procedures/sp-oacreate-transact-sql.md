---
title: sp_OACreate (Transact-SQL) | Documents Microsoft
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
- sp_OACreate
- sp_OACreate_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_OACreate
ms.assetid: eb84c0f1-26dd-48f9-9368-13ee4a30a27c
caps.latest.revision: 32
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 2c7f4598f309549a34cc9dbc39b0ba1a964160bc
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spoacreate-transact-sql"></a>sp_OACreate (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Crée une instance d'un objet OLE.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_OACreate { progid | clsid } , objecttoken OUTPUT [ , context ]   
```  
  
## <a name="arguments"></a>Arguments  
 *ProgID*  
 Identificateur de programme (ProgID) de l'objet OLE à créer. Cette chaîne de caractères décrit la classe de l’objet OLE et se présente sous la forme : **'***OLEComponent***. ***Objet***'**  
  
 *OLEComponent* est le nom du composant du serveur OLE Automation, et *objet* est le nom de l’objet OLE. L’objet OLE spécifié doit être valide et doit prendre en charge la **IDispatch** interface.  
  
 Par exemple, SQLDMO. SQL Server est le ProgID de SQL-DMO **SQLServer** objet. SQL-DMO a un nom de composant SQLDMO, le **SQLServer** objet est valide et (telles que SQL-DMO tous les objets) le **SQLServer** prend en charge de l’objet **IDispatch**.  
  
 *clsid*  
 Identificateur de classe (CLSID) de l'objet OLE à créer. Cette chaîne de caractères décrit la classe de l’objet OLE et se présente sous la forme : **' {***nnnnnnnn-nnnn-nnnn-nnnn-nnnnnnnnnnnn***}'**. L’objet OLE spécifié doit être valide et doit prendre en charge la **IDispatch** interface.  
  
 Par exemple, {00026BA1-0000-0000-C000-000000000046} est le CLSID de SQL-DMO **SQLServer** objet.  
  
 *objecttoken* **sortie**  
 Jeton d’objet retourné, et doit être une variable locale du type de données **int**. Ce jeton d'objet identifie l'objet OLE créé et il est utilisé dans les appels aux autres procédures stockées de OLE Automation.  
  
 *Contexte*  
 Spécifie le contexte d'exécution dans lequel l'objet OLE nouvellement créé s'exécute. Si cet argument est spécifié, il doit avoir une des valeurs suivantes :  
  
 **1** = uniquement serveur OLE de in-process (.dll).  
  
 **4** = local (.exe) OLE serveur uniquement.  
  
 **5** = les serveurs OLE locaux et dans le processus autorisé  
  
 Si non spécifié, la valeur par défaut est **5**. Cette valeur est passée en tant que le *dwClsContext* paramètre de l’appel à **CoCreateInstance**.  
  
 Si un serveur OLE d’in-process est autorisé (à l’aide d’une valeur de contexte de **1** ou **5** ou en ne spécifiant une valeur de contexte), il a accès à la mémoire et aux autres ressources détenues par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Un serveur OLE in-process peut endommager la mémoire ou les ressources [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et provoquer des résultats imprévisibles, comme une violation d'accès [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Lorsque vous spécifiez une valeur de contexte de **4**, un serveur OLE local n’a pas accès à aucun [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ressources et il ne peut pas endommager [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mémoire ou ressources.  
  
> [!NOTE]  
>  Les paramètres de cette procédure stockée sont spécifiés par position, et non pas par nom.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (succès) ou un nombre différent de zéro (échec), qui représente la valeur entière de HRESULT renvoyée par l'objet OLE Automation.  
  
 Pour plus d’informations sur les Codes de retour HRESULT, consultez [OLE Automation Codes de retour et des informations d’erreur](../../relational-databases/stored-procedures/ole-automation-return-codes-and-error-information.md).  
  
## <a name="remarks"></a>Notes  
 Si les procédures OLE automation sont activées, un appel à **sp_OACreate** démarre l’environnement d’exécution partagé de OLE Automation. Pour plus d’informations sur l’activation de OLE automation, consultez [Ole Automation Server Configuration Option procédures](../../database-engine/configure-windows/ole-automation-procedures-server-configuration-option.md).  
  
 L'objet OLE créé est automatiquement détruit à la fin du lot d'instructions [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle serveur fixe **sysadmin** .  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-using-progid"></a>A. Utilisation de ProgID  
 L’exemple suivant crée une SQL-DMO **SQLServer** objet en utilisant son ProgID.  
  
```  
DECLARE @object int;  
DECLARE @hr int;  
DECLARE @src varchar(255), @desc varchar(255);  
EXEC @hr = sp_OACreate 'SQLDMO.SQLServer', @object OUT;  
IF @hr <> 0  
BEGIN  
   EXEC sp_OAGetErrorInfo @object, @src OUT, @desc OUT   
   raiserror('Error Creating COM Component 0x%x, %s, %s',16,1, @hr, @src, @desc)  
    RETURN  
END;  
GO  
```  
  
### <a name="b-using-clsid"></a>B. Utilisation de CLSID  
 L’exemple suivant crée une SQL-DMO **SQLServer** objet en utilisant son CLSID.  
  
```  
DECLARE @object int;  
DECLARE @hr int;  
DECLARE @src varchar(255), @desc varchar(255);  
EXEC @hr = sp_OACreate '{00026BA1-0000-0000-C000-000000000046}',  
    @object OUT;  
IF @hr <> 0  
BEGIN  
   EXEC sp_OAGetErrorInfo @object, @src OUT, @desc OUT   
   raiserror('Error Creating COM Component 0x%x, %s, %s',16,1, @hr, @src, @desc)  
    RETURN  
END;  
GO  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées OLE Automation &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/ole-automation-stored-procedures-transact-sql.md)   
 [Option de Configuration de serveur OLE Automation Procedures](../../database-engine/configure-windows/ole-automation-procedures-server-configuration-option.md)   
 [Exemple de script OLE Automation](../../relational-databases/stored-procedures/ole-automation-sample-script.md)  
  
  
