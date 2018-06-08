---
title: Collection de paramètres (ADO) | Documents Microsoft
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
- Command15::get_Parameters
- Command15::Parameters
- Command15::GetParameters
helpviewer_keywords:
- Parameters collection [ADO]
ms.assetid: 497cae10-3913-422a-9753-dcbb0a639b1b
caps.latest.revision: 20
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 3b9976679454881af9279dc74ff86eacdb01d2a2
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="parameters-collection-ado"></a>Collection de paramètres (ADO)
Contient tous les [paramètre](../../../ado/reference/ado-api/parameter-object.md) les objets d’un [commande](../../../ado/reference/ado-api/command-object-ado.md) objet.  
  
## <a name="remarks"></a>Notes  
 A **commande** objet a un **paramètres** collection composée de **paramètre** objets.  
  
 À l’aide de la [Actualiser](../../../ado/reference/ado-api/refresh-method-ado.md) méthode sur un **commande** l’objet **paramètres** collection récupère les informations de paramètre de fournisseur pour la procédure stockée ou d’une requête paramétrable spécifié dans le **commande** objet. Certains fournisseurs ne gèrent pas les appels de procédures stockées ou des requêtes paramétrées ; appel de la **Actualiser** méthode sur le **paramètres** collection lors de l’utilisation de ce type de fournisseur retournera une erreur.  
  
 Si vous n’avez pas défini votre propre **paramètre** objets et vous accédez à la **paramètres** collection avant d’appeler le **Actualiser** (méthode), ADO appelle automatiquement la méthode et remplir la collection.  
  
 Vous pouvez réduire les appels au fournisseur pour améliorer les performances si vous connaissez les propriétés des paramètres associés à la procédure stockée ou une requête paramétrable que vous souhaitez appeler. Utiliser le [CreateParameter](../../../ado/reference/ado-api/createparameter-method-ado.md) méthode pour créer **paramètre** objets avec les paramètres de propriété appropriés et utilisez la [Append](../../../ado/reference/ado-api/append-method-ado.md) méthode pour les ajouter à la  **Paramètres** collection. Cela vous permet de définir et retourner des valeurs de paramètre sans devoir appeler le fournisseur pour les informations de paramètre. Si vous écrivez un fournisseur qui ne fournit pas d’informations sur les paramètres, vous devez remplir manuellement la **paramètres** collection à l’aide de cette méthode pour pouvoir utiliser des paramètres. Utilisez le [supprimer](../../../ado/reference/ado-api/delete-method-ado-parameters-collection.md) méthode pour supprimer **paramètre** des objets de la **paramètres** collection si nécessaire.  
  
 Les objets dans le **paramètres** collection d’un **Recordset** accédez hors de portée (ils deviennent donc indisponibles) lorsque le **Recordset** est fermé.  
  
 Lorsque vous appelez une procédure stockée avec **commande**, le paramètre de valeur de retour/sortie d’une procédure stockée est extraite comme suit :  
  
1.  Lorsque vous appelez une procédure stockée qui n’a aucun paramètre, le **Actualiser** méthode sur le **paramètres** collection doit être appelée avant d’appeler le **Execute** méthode sur le **Commande** objet.  
  
2.  Lorsque vous appelez une procédure stockée avec des paramètres et explicitement en ajoutant un paramètre à la **paramètres** collection avec **Append**, le paramètre de retour de sortie/valeur doit être ajouté à la **Paramètres** collection. La valeur de retour doit tout d’abord être ajoutée à la **paramètres** collection. Utilisez **Append** pour ajouter les autres paramètres dans le **paramètres** collection dans l’ordre de définition. Par exemple, la procédure stockée SPWithParam a deux paramètres. Le premier paramètre, *InParam*, est un paramètre d’entrée est défini comme adVarChar (20) et le second paramètre, *OutParam*, est un paramètre output défini comme adVarChar (20). Vous pouvez récupérer le paramètre de valeur de retour/sortie par le code suivant.  
  
    ```  
    ' Open Connection Conn  
    set ccmd = CreateObject("ADODB.Command")  
    ccmd.Activeconnection= Conn  
  
    ccmd.CommandText="SPWithParam"  
    ccmd.commandType = 4 'adCmdStoredProc  
  
    ccmd.parameters.Append ccmd.CreateParameter(, adInteger, adParamReturnValue, , NULL)   ' return value  
    ccmd.parameters.Append ccmd.CreateParameter("InParam", adVarChar, adParamInput, 20, "hello world")   ' input parameter  
    ccmd.parameters.Append ccmd.CreateParameter("OutParam", adVarChar, adParamOuput, 20, NULL)   ' output parameter  
  
    ccmd.execute()  
  
    ' Access ccmd.parameters(0) as return value of this stored procedure  
    ' Access ccmd.parameters("OutParam") as the output parameter of this stored procedure.  
  
    ```  
  
3.  Lorsque vous appelez une procédure stockée avec des paramètres et la configuration des paramètres en appelant le **élément** méthode sur le **paramètres** regroupement, le paramètre de valeur de retour/sortie de la procédure stockée peut récupérer à partir de la **paramètres** collection. Par exemple, la procédure stockée SPWithParam a deux paramètres. Le premier paramètre, *InParam*, est un paramètre d’entrée est défini comme adVarChar (20) et le second paramètre, *OutParam*, est un paramètre output défini comme adVarChar (20). Vous pouvez récupérer le paramètre de valeur de retour/sortie par le code suivant.  
  
    ```  
    ' Open Connection Conn  
    set ccmd = CreateObject("ADODB.Command")  
    ccmd.Activeconnection= Conn  
  
    ccmd.CommandText="SPWithParam"  
    ccmd.commandType = 4 'adCmdStoredProc  
  
    ccmd.parameters.Item("InParam").value = "hello world" ' input parameter  
    ccmd.execute()  
  
    ' Access ccmd.parameters(0) as return value of stored procedure  
    ' Access ccmd.parameters(2) or ccmd.parameters("OutParam") as the output parameter.  
    ```  
  
 Cette section contient les rubriques suivantes.  
  
-   [Propriétés de Collection de paramètres, méthodes et événements](../../../ado/reference/ado-api/parameters-collection-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Append (méthode) (ADO)](../../../ado/reference/ado-api/append-method-ado.md)   
 [CreateParameter, méthode (ADO)](../../../ado/reference/ado-api/createparameter-method-ado.md)   
 [Parameter, objet](../../../ado/reference/ado-api/parameter-object.md)
