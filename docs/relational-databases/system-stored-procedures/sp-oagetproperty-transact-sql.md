---
title: sp_OAGetProperty (Transact-SQL) | Documents Microsoft
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
- sp_OAGetProperty_TSQL
- sp_OAGetProperty
dev_langs:
- TSQL
helpviewer_keywords:
- sp_OAGetProperty
ms.assetid: 240eeeb9-6d8b-4930-b912-1d273ca0ab38
caps.latest.revision: 27
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 31b9620e58029285d020371b261dc78cff55b078
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spoagetproperty-transact-sql"></a>sp_OAGetProperty (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Donne une valeur de propriété d'un objet OLE.  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_OAGetProperty objecttoken , propertyname   
    [ , propertyvalue OUTPUT ]  
    [ , index...]   
```  
  
## <a name="arguments"></a>Arguments  
 *objecttoken*  
 Jeton d’objet d’un objet OLE précédemment créé à l’aide de **sp_OACreate**.  
  
 *propertyname*  
 Nom de la propriété de l'objet OLE à renvoyer.  
  
 *PropertyValue* **sortie**  
 Valeur de la propriété renvoyée. Si elle est spécifiée, il doit s'agir d'une variable locale du type de données approprié.  
  
 Si la propriété retourne un objet OLE, *propertyvalue* doit être une variable locale du type de données **int**. Un jeton d'objet est stocké dans la variable locale et il peut être utilisé avec d'autres procédures stockées OLE Automation.  
  
 Si la propriété retourne une valeur unique, soit elle spécifie une variable locale pour *propertyvalue*, qui retourne la propriété de valeur dans la variable locale, ou ne spécifiez pas *propertyvalue*, qui retourne la valeur de propriété au client comme un jeu de résultats d’une seule colonne et d’une ligne unique.  
  
 Lorsque la propriété retourne un tableau, si *propertyvalue* est spécifié, il est défini sur NULL.  
  
 Si *propertyvalue* est spécifié, mais la propriété ne retourne pas une valeur, une erreur se produit. Si la propriété renvoie un tableau qui comprend plus de deux dimensions, une erreur se produit.  
  
 *index*  
 Paramètre d'index. Si spécifié, *index* doit être une valeur de type de données approprié.  
  
 Certaines propriétés possèdent des paramètres. Elles sont désignées sous le nom de propriétés indexées, et les paramètres s'appellent des paramètres d'index. Une propriété peut posséder plusieurs paramètres d'index.  
  
> [!NOTE]  
>  Les paramètres pour cette procédure stockée sont spécifiés par position et non pas par nom.  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 0 (succès) ou un nombre différent de zéro (échec), qui représente la valeur entière de HRESULT renvoyée par l'objet OLE Automation.  
  
 Pour plus d’informations sur les Codes de retour HRESULT, consultez [OLE Automation Codes de retour et des informations d’erreur](../../relational-databases/stored-procedures/ole-automation-return-codes-and-error-information.md).  
  
## <a name="result-sets"></a>Jeux de résultats  
 Si la propriété renvoie un tableau à une ou deux dimensions, le tableau sera renvoyé au client sous la forme d'un jeu de résultats :  
  
-   Un tableau à une dimension est retourné au client sous la forme d'un jeu de résultats d'une seule ligne avec autant de colonnes qu'il y a d'éléments dans le tableau. Autrement dit, le tableau est renvoyé sous forme de colonnes.  
  
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
  
### <a name="a-using-a-local-variable"></a>A. Utilisation d'une variable locale  
 L’exemple suivant obtient la `HostName` propriété (de précédemment créé **SQLServer** objet) et le stocke dans une variable locale.  
  
```  
DECLARE @property varchar(255);  
EXEC @hr = sp_OAGetProperty @object, 'HostName', @property OUT;  
IF @hr <> 0  
BEGIN  
   EXEC sp_OAGetErrorInfo @object  
    RETURN  
END  
PRINT @property;  
```  
  
### <a name="b-using-a-result-set"></a>B. Utilisation d'un jeu de résultats  
 L’exemple suivant obtient la `HostName` propriété (de précédemment créé **SQLServer** objet) et retourne au client dans un jeu de résultats.  
  
```  
EXEC @hr = sp_OAGetProperty @object, 'HostName';  
IF @hr <> 0  
BEGIN  
   EXEC sp_OAGetErrorInfo @object  
    RETURN  
END;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées OLE Automation &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/ole-automation-stored-procedures-transact-sql.md)   
 [Exemple de script OLE Automation](../../relational-databases/stored-procedures/ole-automation-sample-script.md)  
  
  
