---
title: Close (méthode) (ADO) | Documents Microsoft
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
- Recordset15::Close
- _Stream::Close
- _Record::Close
helpviewer_keywords:
- Close method [ADO]
ms.assetid: 3cdf27d1-a180-4cff-8e42-95dec5fb1b55
caps.latest.revision: 12
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 5779f0fdd140ee4fbb95f7d8db339ee4d0b9b8af
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="close-method-ado"></a>Close (méthode) (ADO)
Ferme un objet ouvert et tous les objets dépendants.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
object.Close  
```  
  
## <a name="remarks"></a>Notes  
 Utilisez le **fermer** méthode pour fermer une [connexion](../../../ado/reference/ado-api/connection-object-ado.md), un [enregistrement](../../../ado/reference/ado-api/record-object-ado.md), un [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md), ou un [flux](../../../ado/reference/ado-api/stream-object-ado.md) objet pour libérer les ressources système associées. Fermeture d’un objet ne le supprime pas de la mémoire. Vous pouvez modifier ses paramètres de propriété et ouvrez à nouveau ultérieurement. Pour éliminer définitivement un objet de la mémoire, fermez l’objet et ensuite définir la variable objet *rien* (en Visual Basic).  
  
## <a name="connection"></a>Connexion  
 À l’aide de la **fermer** méthode pour fermer une **connexion** objet ferme également toutes les **Recordset** objets associés à la connexion. A [commande](../../../ado/reference/ado-api/command-object-ado.md) objet associé à la **connexion** objet que vous fermez est conservé, mais il sera n’est plus associé avec un **connexion** objet ; autrement dit, son [ ActiveConnection](../../../ado/reference/ado-api/activeconnection-property-ado.md) propriété sera définie **rien**. En outre, le **commande** l’objet [paramètres](../../../ado/reference/ado-api/parameters-collection-ado.md) est supprimée de tous les paramètres définis sur le fournisseur de la collection.  
  
 Vous pouvez appeler ultérieurement la [ouvrir](../../../ado/reference/ado-api/open-method-ado-connection.md) méthode afin de rétablir la connexion à la même source de données ou à une source de données différente. Alors que le **connexion** objet est fermé, l’appel d’une méthode nécessitant une connexion ouverte à la source de données génère une erreur.  
  
 Fermeture un **connexion** lorsqu’il y a ouvert de l’objet **Recordset** objets sur la connexion annule toutes les modifications en attente dans tous les **Recordset** objets. Fermer explicitement un **connexion** objet (appelant le **fermer** méthode) lorsqu’une transaction est en cours génère une erreur. Si un **connexion** objet devient hors de portée lorsqu’une transaction est en cours, ADO restaure automatiquement la transaction.  
  
## <a name="recordset-record-stream"></a>Jeu d’enregistrements, l’enregistrement, de flux  
 À l’aide de la **fermer** méthode pour fermer une **Recordset**, **enregistrement**, ou **flux** objet libère les données associées et un accès exclusif vous disposiez aux données via cet objet spécifique. Vous pouvez appeler ultérieurement la [ouvrir](../../../ado/reference/ado-api/open-method-ado-recordset.md) méthode pour rouvrir l’objet avec le même ou modifiée, les attributs.  
  
 Pendant un **Recordset** objet est fermé, l’appel d’une méthode nécessitant un curseur actif génère une erreur.  
  
 Si une modification est en cours d’exécution en mode de mise à jour immédiate, l’appel du **fermer** méthode génère une erreur ; au lieu de cela, appelez le [mettre à jour](../../../ado/reference/ado-api/update-method.md) ou [CancelUpdate](../../../ado/reference/ado-api/cancelupdate-method-ado.md) méthode premier. Si vous fermez le **Recordset** de l’objet en mode de mise à jour par lot, toutes les modifications depuis le dernier [UpdateBatch](../../../ado/reference/ado-api/updatebatch-method.md) appel sont perdues.  
  
 Si vous utilisez la [Clone](../../../ado/reference/ado-api/clone-method-ado.md) méthode pour créer des copies ouvert **Recordset** l’objet, la fermeture de la version d’origine ou un clone n’affecte pas les autres copies.  
  
## <a name="applies-to"></a>S'applique à  
  
|||  
|-|-|  
|[Connection, objet (ADO MD)](../../../ado/reference/ado-api/connection-object-ado.md)|[Record, objet (ADO)](../../../ado/reference/ado-api/record-object-ado.md)|  
|[Recordset, objet (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md)|[Stream, objet (ADO)](../../../ado/reference/ado-api/stream-object-ado.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Exemple de méthodes d’ouverture et de fermeture (VB)](../../../ado/reference/ado-api/open-and-close-methods-example-vb.md)   
 [Les méthodes Open et Close-exemple (VBScript)](../../../ado/reference/ado-api/open-and-close-methods-example-vbscript.md)   
 [Les méthodes Open et Close-exemple (VC ++)](../../../ado/reference/ado-api/open-and-close-methods-example-vc.md)   
 [Open (méthode) (connexion ADO)](../../../ado/reference/ado-api/open-method-ado-connection.md)   
 [Open (méthode) (jeu d’enregistrements ADO)](../../../ado/reference/ado-api/open-method-ado-recordset.md)   
 [Save, méthode](../../../ado/reference/ado-api/save-method.md)
