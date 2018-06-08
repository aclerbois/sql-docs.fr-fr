---
title: Open (méthode) (connexion ADO) | Documents Microsoft
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
- Connection15::raw_Open
- Connection15::Open
- _Connection::Open
helpviewer_keywords:
- Open method [ADO]
ms.assetid: 663defab-5545-4973-9036-24d5882c9737
caps.latest.revision: 13
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 252afc6de9b6cf405fba7ae21a191beef2c198e7
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="open-method-ado-connection"></a>Open (méthode) (connexion ADO)
Ouvre une connexion à une source de données.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
connection.Open ConnectionString, UserID, Password, Options  
```  
  
#### <a name="parameters"></a>Paramètres  
 *connectionString*  
 Ce paramètre est facultatif. A **chaîne** valeur qui contient des informations de connexion. Consultez le [ConnectionString](../../../ado/reference/ado-api/connectionstring-property-ado.md) pour plus d’informations sur les paramètres corrects.  
  
 *ID d’utilisateur*  
 Ce paramètre est facultatif. A **chaîne** valeur qui contient un nom d’utilisateur à utiliser pour établir la connexion.  
  
 *Mot de passe*  
 Ce paramètre est facultatif. A **chaîne** valeur qui contient un mot de passe à utiliser pour établir la connexion.  
  
 *Options*  
 Ce paramètre est facultatif. A [ConnectOptionEnum](../../../ado/reference/ado-api/connectoptionenum.md) valeur qui détermine si cette méthode doit retourner après (synchrone) ou avant (de façon asynchrone) la connexion est établie.  
  
## <a name="remarks"></a>Notes  
 À l’aide de la **ouvrir** méthode sur un [connexion](../../../ado/reference/ado-api/connection-object-ado.md) objet établit la connexion physique à une source de données. Une fois que cette méthode se termine correctement, la connexion est établie et vous pouvez émettre des commandes sur elle et traiter les résultats.  
  
 Utilisez le paramètre facultatif *ConnectionString* argument afin de spécifier une chaîne de connexion contenant une série de *argument* *= valeur* instructions séparées par des points-virgules, ou un fichier ou répertoire ressource identifiée par une URL. Le **ConnectionString** propriété hérite automatiquement de la valeur utilisée pour le *ConnectionString* argument. Par conséquent, vous pouvez soit définir la **ConnectionString** propriété de la **connexion** de l’objet avant de l’ouvrir, ou utilisez le *ConnectionString* argument pour définir ou substituer les paramètres de connexion en cours pendant le **ouvrir** appel de méthode.  
  
 Si vous transmettez des informations utilisateur et mot de passe à la fois dans le *ConnectionString* argument et le paramètre facultatif *UserID* et *mot de passe* arguments, le *UserID*  et *mot de passe* arguments remplaceront les valeurs spécifiées dans *ConnectionString*.  
  
 Lorsque vous avez terminé les opérations au moyen d’un **connexion**, utilisez le [fermer](../../../ado/reference/ado-api/close-method-ado.md) méthode pour libérer les ressources système d’associées. Fermeture d’un objet ne le supprime pas de la mémoire. Vous pouvez modifier ses paramètres de propriété et utiliser la **ouvrir** méthode pour l’ouvrir à nouveau ultérieurement. Pour éliminer définitivement un objet de la mémoire, affectez la variable objet *rien*.  
  
> [!NOTE]
>  **Utilisation du Service de données à distance** lorsqu’il est utilisé sur un côté client **connexion** objet, le **ouvrir** méthode réellement n’établit pas une connexion au serveur jusqu'à ce qu’une [Recordset ](../../../ado/reference/ado-api/recordset-object-ado.md) est ouvert sur le **connexion** objet.  
  
> [!NOTE]
>  URL à l’aide du modèle http appellent automatiquement le [fournisseur Microsoft OLE DB pour la publication Internet](../../../ado/guide/appendixes/microsoft-ole-db-provider-for-internet-publishing.md). Pour plus d’informations, consultez [URL absolues et relatives](../../../ado/guide/data/absolute-and-relative-urls.md).  
  
## <a name="applies-to"></a>S'applique à  
 [Connection, objet (ADO MD)](../../../ado/reference/ado-api/connection-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Exemple de méthodes d’ouverture et de fermeture (VB)](../../../ado/reference/ado-api/open-and-close-methods-example-vb.md)   
 [Les méthodes Open et Close-exemple (VBScript)](../../../ado/reference/ado-api/open-and-close-methods-example-vbscript.md)   
 [Les méthodes Open et Close-exemple (VC ++)](../../../ado/reference/ado-api/open-and-close-methods-example-vc.md)   
 [Open (méthode) (enregistrement ADO)](../../../ado/reference/ado-api/open-method-ado-record.md)   
 [Open (méthode) (jeu d’enregistrements ADO)](../../../ado/reference/ado-api/open-method-ado-recordset.md)   
 [Open (méthode) (flux ADO)](../../../ado/reference/ado-api/open-method-ado-stream.md)   
 [OpenSchema, méthode](../../../ado/reference/ado-api/openschema-method.md)
