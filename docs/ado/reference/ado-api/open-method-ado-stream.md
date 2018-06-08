---
title: Open (méthode) (flux ADO) | Documents Microsoft
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
- _Stream::raw_Open
- _Stream::Open
helpviewer_keywords:
- Open method [ADO]
ms.assetid: d26f48fb-904e-4932-a245-3b4332ca1600
caps.latest.revision: 15
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 44493854720564e241817c1b482339f9c5cbbd32
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="open-method-ado-stream"></a>Open (méthode) (flux ADO)
Ouvre un [flux](../../../ado/reference/ado-api/stream-object-ado.md) objet à manipuler des flux de données binaire ou texte.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Stream.Open Source, Mode , OpenOptions, UserName, Password  
```  
  
#### <a name="parameters"></a>Paramètres  
 *Source*  
 Ce paramètre est facultatif. A **Variant** valeur qui spécifie la source de données pour le **flux**. *Source* peut contenir une chaîne d’URL absolue qui pointe vers un nœud existant dans une arborescence connue, par exemple un système de fichiers ou de messagerie. Une URL doit être spécifiée à l’aide du mot clé URL (« URL =*schéma*://*server*/*dossier*»). Vous pouvez également *Source* peut contenir une référence à un déjà ouvert [enregistrement](../../../ado/reference/ado-api/record-object-ado.md) objet, qui ouvre le flux par défaut associé à la **enregistrement**. Si *Source* n’est pas spécifié, un **flux** est instancié et ouvert, associé à aucune source sous-jacente par défaut. Pour plus d’informations sur les schémas d’URL et de leurs fournisseurs associés, consultez [URL absolues et relatives](../../../ado/guide/data/absolute-and-relative-urls.md).  
  
 *Mode*  
 Ce paramètre est facultatif. A [ConnectModeEnum](../../../ado/reference/ado-api/connectmodeenum.md) valeur qui spécifie le mode d’accès pour le résultant **flux** (par exemple, en lecture/écriture ou en lecture seule). Valeur par défaut est **adModeUnknown**. Consultez le [Mode](../../../ado/reference/ado-api/mode-property-ado.md) propriété pour plus d’informations sur les modes d’accès. Si *Mode* n’est pas spécifié, il est hérité par l’objet source. Par exemple, si la source de **enregistrement** est ouvert en mode lecture seule, le **flux** sera également ouvert en mode lecture seule par défaut.  
  
 *OpenOptions*  
 Ce paramètre est facultatif. A [StreamOpenOptionsEnum](../../../ado/reference/ado-api/streamopenoptionsenum.md) valeur. Valeur par défaut est **adOpenStreamUnspecified**.  
  
 *UserName*  
 Ce paramètre est facultatif. A **chaîne** valeur qui contient l’ID d’utilisateur qui, s’il est nécessaire, accède à la **flux** objet.  
  
 *Mot de passe*  
 Ce paramètre est facultatif. A **chaîne** valeur qui contient le mot de passe, s’il est nécessaire, accède à la **flux** objet.  
  
## <a name="remarks"></a>Notes  
 Lorsqu’un **enregistrement** objet est passé comme paramètre source, le *UserID* et *mot de passe* paramètres ne sont pas utilisés, car l’accès à la **enregistrement** objet est déjà disponible. De même, la [Mode](../../../ado/reference/ado-api/mode-property-ado.md) de la **enregistrement** objet est transféré vers le **flux** objet. Lorsque *Source* n’est pas spécifié, le **flux** ouvert ne contient aucune donnée et a un [taille](../../../ado/reference/ado-api/size-property-ado-stream.md) de zéro (0). Pour éviter de perdre des données qui sont écrit à ce **flux** lors de la **flux** est fermée, enregistrer le **flux** avec la [CopyTo](../../../ado/reference/ado-api/copyto-method-ado.md) ou [ SaveToFile](../../../ado/reference/ado-api/savetofile-method.md) méthodes, ou l’enregistrer dans un autre emplacement de mémoire.  
  
 Un *si OptionsOuverture a* valeur **adOpenStreamFromRecord** identifie le contenu de la *Source* le paramètre doit être déjà ouvert **enregistrement**objet. Le comportement par défaut consiste à traiter *Source* comme une URL qui pointe directement vers un nœud dans une arborescence, telle qu’un fichier. Le flux par défaut associé à ce nœud est ouvert.  
  
 Alors que le **flux** est pas ouvert, il est possible de lire toutes les propriétés en lecture seule de la **flux**. Si un **flux** est ouvert de façon asynchrone, toutes les opérations suivantes (autres que la vérification le [état](../../../ado/reference/ado-api/state-property-ado.md) et d’autres propriétés en lecture seule) sont bloquées jusqu'à ce que le **ouvrir** opération terminée.  
  
 Outre les options mentionnées précédemment, en ne spécifiant *Source*, vous pouvez créer une instance d’un **flux** objet en mémoire sans l’associer à une source sous-jacente. Vous pouvez ajouter dynamiquement des données dans le flux en écrivant des données binaires ou de texte à la **flux** avec [écrire](../../../ado/reference/ado-api/write-method.md) ou [WriteText](../../../ado/reference/ado-api/writetext-method.md), ou en chargeant les données à partir d’un fichier avec [ LoadFromFile](../../../ado/reference/ado-api/loadfromfile-method-ado.md).  
  
## <a name="applies-to"></a>S'applique à  
 [Stream, objet (ADO)](../../../ado/reference/ado-api/stream-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Open (méthode) (connexion ADO)](../../../ado/reference/ado-api/open-method-ado-connection.md)   
 [Open (méthode) (enregistrement ADO)](../../../ado/reference/ado-api/open-method-ado-record.md)   
 [Open (méthode) (jeu d’enregistrements ADO)](../../../ado/reference/ado-api/open-method-ado-recordset.md)   
 [OpenSchema, méthode](../../../ado/reference/ado-api/openschema-method.md)   
 [SaveToFile, méthode](../../../ado/reference/ado-api/savetofile-method.md)
