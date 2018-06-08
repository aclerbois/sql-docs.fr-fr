---
title: Execute (méthode) (commande ADO) | Documents Microsoft
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
- Command15::Execute
- Command15::raw_Execute
helpviewer_keywords:
- Execute method [ADO]
ms.assetid: f84a5ff3-0528-4ad7-9bea-9a15103378dd
caps.latest.revision: 13
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 8936219aa3d8e75a43efcc51936ac23916c51d96
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="execute-method-ado-command"></a>Execute (méthode) (commande ADO)
Exécute la requête, une instruction SQL ou une procédure stockée spécifiée dans le [CommandText](../../../ado/reference/ado-api/commandtext-property-ado.md) ou [CommandStream](../../../ado/reference/ado-api/commandstream-property-ado.md) propriété de la [objet de commande](../../../ado/reference/ado-api/command-object-ado.md).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Set recordset = command.Execute( RecordsAffected, Parameters, Options )  
```  
  
## <a name="return-value"></a>Valeur retournée  
 Retourne un [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) référence d’objet, d’un flux, ou **rien**.  
  
#### <a name="parameters"></a>Paramètres  
 *RecordsAffected*  
 Ce paramètre est facultatif. A **Long** variable sur laquelle le fournisseur retourne le nombre d’enregistrements affectée par l’opération. Le *RecordsAffected* paramètre s’applique uniquement pour les requêtes d’action ou des procédures stockées. *RecordsAffected* ne retourne pas le nombre d’enregistrements retournés par une procédure stockée ou une requête qui retourne des résultats. Pour obtenir ces informations, utilisez le [RecordCount](../../../ado/reference/ado-api/recordcount-property-ado.md) propriété. Le **Execute** méthode ne retourne pas les informations correctes en cas d’utilisation avec **adAsyncExecute**, simplement parce que lorsqu’une commande est exécutée de façon asynchrone, le nombre d’enregistrements concernés peut encore être inconnue au moment où la méthode retourne.  
  
 *Paramètres*  
 Ce paramètre est facultatif. A **Variant** tableau de valeurs de paramètre utilisée conjointement avec la chaîne d’entrée ou le flux spécifié dans **CommandText** ou **CommandStream**. (Les paramètres de sortie ne retournent pas de valeurs correctes lorsqu’il est passé dans cet argument.)  
  
 *Options*  
 Ce paramètre est facultatif. A **Long** valeur qui indique la manière dont le fournisseur doit évaluer le [CommandText](../../../ado/reference/ado-api/commandtext-property-ado.md) ou [CommandStream](../../../ado/reference/ado-api/commandstream-property-ado.md) propriété de la [commande](../../../ado/reference/ado-api/command-object-ado.md) objet. Peut être une valeur de masque de bits à l’aide de [CommandTypeEnum](../../../ado/reference/ado-api/commandtypeenum.md) et/ou [ExecuteOptionEnum](../../../ado/reference/ado-api/executeoptionenum.md) valeurs. Par exemple, vous pouvez utiliser **adCmdText** et **adExecuteStream** en combinaison, si vous voulez qu’ADO évalue la valeur de la **CommandText** propriété sous forme de texte, et indiquer que la commande doit ignorer et ne retourne pas les enregistrements qui peuvent être générés lorsque le texte de commande s’exécute.  
  
> [!NOTE]
>  Utilisez le **ExecuteOptionEnum** valeur **adExecuteStream** pour améliorer les performances en réduisant le traitement interne. Si **adExecuteStream** a été spécifié, les options **adAsyncFetch** et **adAsynchFetchNonBlocking** sont ignorés. N’utilisez pas le **CommandTypeEnum** les valeurs de **adCmdFile** ou **adCmdTableDirect** avec **Execute**. Ces valeurs peuvent uniquement être utilisées en tant qu’options avec les [ouvrir](../../../ado/reference/ado-api/open-method-ado-recordset.md) et [Requery](../../../ado/reference/ado-api/requery-method.md) méthodes d’un **Recordset**.  
  
## <a name="remarks"></a>Notes  
 À l’aide de la **Execute** méthode sur un **commande** objet exécute la requête spécifiée dans le **CommandText** propriété ou **CommandStream** propriété de l’objet.  
  
 Résultats sont retournés dans un **Recordset** (par défaut) ou en tant que flux d’informations binaires. Pour obtenir un flux binaire, spécifiez **adExecuteStream** dans *Options*, puis fournir un flux de données en définissant **Command.Properties (« flux de sortie »)**. ADO **flux** objet peut être spécifié pour recevoir les résultats, ou un autre objet de flux de données tels que l’objet de réponse de IIS peut être spécifié. Si aucun flux n’a été spécifié avant d’appeler **Execute** avec **adExecuteStream**, une erreur se produit. La position du flux de données lors du retour de **Execute** est le fournisseur spécifique.  
  
 Si la commande ne vise pas à retourner des résultats (par exemple, une requête SQL UPDATE) le fournisseur retourne **rien** tant que l’option **adExecuteStream** est spécifiée, sinon exécutez retourne un fermé **Recordset**. Certains langages d’application vous permettent d’ignorer cette valeur de retour si aucun **Recordset** est souhaitée.  
  
 **Exécutez** génère une erreur si l’utilisateur spécifie une valeur pour **CommandStream** lors de la **CommandType** est **valeur adCmdStoredProc**,  **adCmdTable**, ou **adCmdTableDirect**.  
  
 Si la requête possède des paramètres, les valeurs en cours pour le **commande** les paramètres de l’objet sont utilisés sauf si vous remplacez les valeurs de paramètre passées avec le **Execute** appeler. Vous pouvez remplacer un sous-ensemble des paramètres en omettant les nouvelles valeurs pour certains des paramètres lors de l’appel du **Execute** (méthode). L’ordre dans lequel vous spécifiez les paramètres est le même ordre que celui dans lequel la méthode les passe. Par exemple, si les paramètres de quatre (ou plus) et vous souhaitez passer de nouvelles valeurs uniquement pour le premier et le quatrième, vous devez passer `Array(var1,,,var4)` comme le *paramètres* argument.  
  
> [!NOTE]
>  Paramètres de sortie ne retournent pas de valeurs correctes lorsqu’il est passé dans le *paramètres* argument.  
  
 Un [ExecuteComplete](../../../ado/reference/ado-api/executecomplete-event-ado.md) événement est émis lorsque cette opération se termine.  
  
> [!NOTE]
>  Lors de l’émission des commandes contenant l’URL, ceux qui utilisent le modèle http appellent automatiquement le [fournisseur Microsoft OLE DB pour la publication Internet](../../../ado/guide/appendixes/microsoft-ole-db-provider-for-internet-publishing.md). Pour plus d’informations, consultez [URL absolues et relatives](../../../ado/guide/data/absolute-and-relative-urls.md).  
  
## <a name="applies-to"></a>S'applique à  
 [Command, objet (ADO)](../../../ado/reference/ado-api/command-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Execute, Requery et Clear, méthodes-exemple (VB)](../../../ado/reference/ado-api/execute-requery-and-clear-methods-example-vb.md)   
 [Execute, Requery et Clear, méthodes-exemple (VBScript)](../../../ado/reference/ado-api/execute-requery-and-clear-methods-example-vbscript.md)   
 [Execute, Requery et Clear, méthodes-exemple (VC ++)](../../../ado/reference/ado-api/execute-requery-and-clear-methods-example-vc.md)   
 [CommandStream, propriété (ADO)](../../../ado/reference/ado-api/commandstream-property-ado.md)   
 [CommandText, propriété (ADO)](../../../ado/reference/ado-api/commandtext-property-ado.md)   
 [CommandTypeEnum](../../../ado/reference/ado-api/commandtypeenum.md)   
 [Execute (méthode) (connexion ADO)](../../../ado/reference/ado-api/execute-method-ado-connection.md)   
 [ExecuteComplete, événement (ADO)](../../../ado/reference/ado-api/executecomplete-event-ado.md)
