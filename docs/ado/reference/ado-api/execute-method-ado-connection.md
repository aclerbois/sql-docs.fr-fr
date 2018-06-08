---
title: Execute (méthode) (connexion ADO) | Documents Microsoft
ms.prod: sql
ms.prod_service: connectivity
ms.component: ado
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.suite: sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
apitype: COM
f1_keywords:
- Connection15::Execute
- Connection15::raw_Execute
helpviewer_keywords:
- Execute method [ADO]
ms.assetid: 03c69320-96b2-4d85-8d49-a13b13e31578
caps.latest.revision: 13
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 27f99015571bd7abdad402dc0f779c04fd546a79
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="execute-method-ado-connection"></a>Execute (méthode) (connexion ADO)
Exécute la requête spécifiée, l’instruction SQL, une procédure stockée ou du texte spécifique au fournisseur.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Set recordset = connection.Execute (CommandText, RecordsAffected, Options)  
Set recordset = connection.Execute (CommandText, RecordsAffected, Options)  
```  
  
## <a name="return-value"></a>Valeur retournée  
 Retourne un [objet Recordset (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md) référence d’objet.  
  
#### <a name="parameters"></a>Paramètres  
 *commandText*  
 A **chaîne** valeur qui contient l’instruction SQL, procédure stockée, une URL ou spécifiques au fournisseur du texte à exécuter. **Si vous le souhaitez**, noms de table peuvent être utilisés uniquement si le fournisseur est compatible SQL. Exemple si un nom de la table « Customers » est utilisé, ADO ajoute automatiquement la syntaxe SQL Select standard pour créer et de passer « SELECT * FROM Customers » en tant qu’un [!INCLUDE[tsql](../../../includes/tsql_md.md)] instruction au fournisseur.  
  
 *RecordsAffected*  
 Ce paramètre est facultatif. A **Long** variable sur laquelle le fournisseur retourne le nombre d’enregistrements affectée par l’opération.  
  
 *Options*  
 Ce paramètre est facultatif. A **Long** valeur qui indique la manière dont le fournisseur doit évaluer l’argument CommandText. Peut être un masque de bits d’une ou plusieurs [CommandTypeEnum](../../../ado/reference/ado-api/commandtypeenum.md) ou [ExecuteOptionEnum](../../../ado/reference/ado-api/executeoptionenum.md) valeurs.  
  
 **Remarque** utilisez la **ExecuteOptionEnum** valeur **adExecuteStream** pour améliorer les performances en réduisant le traitement interne et pour les applications que vous portez à partir de Visual Basic 6.0.  
  
 N’utilisez pas **adExecuteStream** avec la **Execute** méthode d’un **connexion** objet.  
  
 N’utilisez pas les valeurs CommandTypeEnum adCmdFile ou adCmdTableDirect avec Execute. Ces valeurs peuvent uniquement être utilisées en tant qu’options avec la [Open (méthode) (jeu d’enregistrements ADO)](../../../ado/reference/ado-api/open-method-ado-recordset.md) et [méthode Requery](../../../ado/reference/ado-api/requery-method.md) méthodes d’un **Recordset**.  
  
## <a name="remarks"></a>Notes  
 À l’aide de la **Execute** méthode sur un [connexion Object (ADO)](../../../ado/reference/ado-api/connection-object-ado.md) objet exécute toute requête que vous passez à la méthode dans l’argument CommandText sur la connexion spécifiée. Si l’argument CommandText spécifie une requête retourne des lignes, les résultats générés par l’exécution sont stockés dans une nouvelle **Recordset** objet. Si la commande ne vise pas à retourner des résultats (par exemple, une requête SQL UPDATE) le fournisseur retourne **rien** tant que l’option **adExecuteStream** est spécifiée, sinon exécutez retourne un fermé **Recordset**.  
  
 Retourné **Recordset** objet est toujours un curseur avant uniquement en lecture seule. Si vous avez besoin d’un **Recordset** objet avec davantage de fonctionnalités, commencez par créer un **Recordset** de l’objet avec les paramètres de propriété de votre choix, puis utilisez le **Recordset** l’objet[ Open (méthode) (jeu d’enregistrements ADO)](../../../ado/reference/ado-api/open-method-ado-recordset.md) méthode à exécuter la requête et retourner le type de curseur souhaité.  
  
 Le contenu de la *CommandText* argument sont spécifique au fournisseur et peut être n’importe quel format de commande spécial qui prend en charge par le fournisseur ou de la syntaxe SQL standard.  
  
 Un événement ExecuteComplete est déclenché lorsque cette opération se termine.  
  
> [!NOTE]
>  URL à l’aide du modèle http appellent automatiquement le [fournisseur Microsoft OLE DB pour la publication Internet](../../../ado/guide/appendixes/microsoft-ole-db-provider-for-internet-publishing.md). Pour plus d’informations, consultez [URL absolues et relatives](../../../ado/guide/data/absolute-and-relative-urls.md).  
  
## <a name="applies-to"></a>S'applique à  
 [Connection, objet (ADO MD)](../../../ado/reference/ado-api/connection-object-ado.md)
