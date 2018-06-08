---
title: sp_OAMethod (Transact-SQL) | Documents Microsoft
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
- sp_OAMethod
- sp_OAMethod_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_OAMethod
ms.assetid: 1dfaebe2-c7cf-4041-a586-5d04faf2e25e
caps.latest.revision: 25
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: ef5d79a14aaee0b8f23738c3e359e37032d80318
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spoamethod-transact-sql"></a>sp_OAMethod (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Appelle une méthode d'un objet OLE.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_OAMethod objecttoken , methodname  
    [ , returnvalue OUTPUT ]   
    [ , [ @parametername = ] parameter [ OUTPUT ] [ ...n ] ]   
```  
  
## <a name="arguments"></a>Arguments  
 *objecttoken*  
 Jeton d’objet d’un objet OLE précédemment créé à l’aide de **sp_OACreate**.  
  
 *MethodName*  
 Nom de la méthode de l'objet OLE à appeler.  
  
 *ReturnValue***sortie**  
 Valeur renvoyée de la méthode de l'objet OLE. Si elle est spécifiée, il doit s'agir d'une variable locale du type de données approprié.  
  
 Si la méthode retourne une valeur unique, soit elle spécifie une variable locale pour *returnvalue*, qui retourne la méthode retourne la valeur dans la variable locale ou ne spécifiez pas *returnvalue*, qui retourne la valeur de retour de méthode au client comme un jeu de résultats d’une seule colonne et d’une ligne unique.  
  
 Si la méthode retourne la valeur est un objet OLE, *returnvalue* doit être une variable locale du type de données **int**. Un jeton d'objet est stocké dans la variable locale et il peut être utilisé avec d'autres procédures stockées OLE Automation.  
  
 Lorsque la méthode de valeur de retour est un tableau, si *returnvalue* est spécifié, il est défini sur NULL.  
  
 Une erreur est générée lorsqu'un des événements suivants survient :  
  
-   *ReturnValue* est spécifié, mais la méthode ne retourne pas de valeur.  
  
-   La méthode renvoie un tableau qui possède plus de deux dimensions.  
  
-   La méthode renvoie un tableau comme paramètre de sortie.  
  
 [  *@parametername*** =**] *paramètre*[ **sortie** ]  
 Paramètre de la méthode. Si spécifié, *paramètre* doit être une valeur de type de données approprié.  
  
 Pour obtenir la valeur de retour d’un paramètre de sortie, *paramètre* doit être une variable locale du type de données approprié, et **sortie** doit être spécifié. Si un paramètre constant est spécifié ou si **sortie** n’est pas spécifié, les retournent la valeur à partir d’un paramètre de sortie est ignorée.  
  
 Si spécifié, *nom_paramètre* doit être le nom de la [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] paramètre nommé. Notez que  **@** *nom_paramètre*n’est pas un [!INCLUDE[tsql](../../includes/tsql-md.md)] variable locale. Le signe arobase (**@**) est supprimé, et *nom_paramètre*est passé à l’objet OLE en tant que le nom du paramètre. Tous les paramètres nommés doivent être spécifiés après tous les paramètres positionnels.  
  
 *n*  
 Marque de réservation indiquant que plusieurs paramètres peuvent être spécifiés.  
  
> [!NOTE]  
>  *@parametername* possible un paramètre nommé car il fait partie de la méthode spécifiée et est transmis à l’objet. Les autres paramètres pour cette procédure stockée sont spécifiés par position, et non par nom.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (succès) ou un nombre différent de zéro (échec), qui représente la valeur entière de HRESULT renvoyée par l'objet OLE Automation.  
  
 Pour plus d’informations sur les Codes de retour HRESULT, [OLE Automation Codes de retour et des informations d’erreur](../../relational-databases/stored-procedures/ole-automation-return-codes-and-error-information.md).  
  
## <a name="result-sets"></a>Jeux de résultats  
 Si la valeur renvoyée par la méthode est un tableau à une ou deux dimensions, le tableau sera renvoyé au client sous la forme d'un jeu de résultats :  
  
-   Un tableau à une dimension est retourné au client sous la forme d'un jeu de résultats d'une seule ligne avec autant de colonnes qu'il y a d'éléments dans le tableau. En d’autres termes, le tableau est renvoyé en tant que (colonnes).  
  
-   Un tableau à deux dimensions est retourné au client sous la forme d'un jeu de résultats qui contient autant de colonnes qu'il y a d'éléments dans la première dimension du tableau et autant de lignes qu'il y a d'éléments dans la seconde dimension du tableau. Autrement dit, le tableau est renvoyé sous la forme (colonnes, lignes).  
  
 Lorsque la valeur de retour d’une propriété ou valeur de retour de méthode est un tableau, **sp_OAGetProperty** ou **sp_OAMethod** renvoie un jeu de résultats pour le client. (Les paramètres de sortie de la méthode ne peuvent pas être des tableaux). Ces procédures analysent toutes les valeurs de données dans le tableau pour déterminer les types de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et les longueurs de données appropriés à utiliser pour chaque colonne du jeu de résultats. Pour une colonne particulière, ces procédures utilisent le type de données et la longueur requis pour représenter toutes les valeurs des données de cette colonne.  
  
 Lorsque toutes les valeurs de données d'une colonne partagent le même type de données, ce type est utilisé pour toute la colonne. Lorsque les valeurs de données d'une colonne utilisent des types de données différents, le choix du type pour l'ensemble de la colonne se fait sur la base du tableau suivant.  
  
||int|float|money|datetime|varchar|nvarchar|  
|------|---------|-----------|-----------|--------------|-------------|--------------|  
|**int**|**int**|**float**|**money**|**varchar**|**varchar**|**nvarchar**|  
|**float**|**float**|**float**|**money**|**varchar**|**varchar**|**nvarchar**|  
|**money**|**money**|**money**|**money**|**varchar**|**varchar**|**nvarchar**|  
|**datetime**|**varchar**|**varchar**|**varchar**|**datetime**|**varchar**|**nvarchar**|  
|**varchar**|**varchar**|**varchar**|**varchar**|**varchar**|**varchar**|**nvarchar**|  
|**nvarchar**|**nvarchar**|**nvarchar**|**nvarchar**|**nvarchar**|**nvarchar**|**nvarchar**|  
  
## <a name="remarks"></a>Notes  
 Vous pouvez également utiliser **sp_OAMethod** pour obtenir une valeur de propriété.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'appartenance au rôle serveur fixe **sysadmin** .  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-calling-a-method"></a>A. Appel d'une méthode  
 L’exemple suivant appelle la `Connect` méthode précédemment créé **SQLServer** objet.  
  
```  
EXEC @hr = sp_OAMethod @object, 'Connect', NULL, 'my_server',  
    'my_login', 'my_password';  
IF @hr <> 0  
BEGIN  
   EXEC sp_OAGetErrorInfo @object  
    RETURN  
END;  
```  
  
### <a name="b-getting-a-property"></a>B. Obtention d'une propriété  
 L’exemple suivant obtient la `HostName` propriété (de précédemment créé **SQLServer** objet) et le stocke dans une variable locale.  
  
```  
DECLARE @property varchar(255);  
EXEC @hr = sp_OAMethod @object, 'HostName', @property OUT;  
IF @hr <> 0  
BEGIN  
   EXEC sp_OAGetErrorInfo @object  
    RETURN  
END;  
PRINT @property;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées OLE Automation &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/ole-automation-stored-procedures-transact-sql.md)   
 [Exemple de script OLE Automation](../../relational-databases/stored-procedures/ole-automation-sample-script.md)  
  
  
