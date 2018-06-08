---
title: Open (méthode) (enregistrement ADO) | Documents Microsoft
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
- _Record::raw_Open
- _Record::Open
helpviewer_keywords:
- Open method [ADO]
ms.assetid: ab79a623-88a9-40b6-a017-a658bf19b778
caps.latest.revision: 15
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 432f2a821bd276efd46f61497f4ef7ae95502bf7
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="open-method-ado-record"></a>Open (méthode) (enregistrement ADO)
Ouvre une existante [enregistrement](../../../ado/reference/ado-api/record-object-ado.md) de l’objet ou crée un nouvel élément représenté par le **enregistrement**, par exemple un fichier ou répertoire.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
Open Source, ActiveConnection, Mode, CreateOptions, Options, UserName, Password  
```  
  
#### <a name="parameters"></a>Paramètres  
 *Source*  
 Ce paramètre est facultatif. A **Variant** qui peut représenter l’URL de l’entité pour être représentée par ce **enregistrement** objet, un **commande**, open [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) ou un autre **enregistrement** (objet), une chaîne qui contient une instruction SQL SELECT ou un nom de table.  
  
 *ActiveConnection*  
 Ce paramètre est facultatif. A **Variant** qui représente la chaîne de connexion ou ouvrez [connexion](../../../ado/reference/ado-api/connection-object-ado.md) objet.  
  
 *Mode*  
 Ce paramètre est facultatif. A [ConnectModeEnum](../../../ado/reference/ado-api/connectmodeenum.md) valeur qui spécifie le mode d’accès pour le résultant **enregistrement** objet. Valeur par défaut est **adModeUnknown**.  
  
 *CreateOptions*  
 Ce paramètre est facultatif. A [RecordCreateOptionsEnum](../../../ado/reference/ado-api/recordcreateoptionsenum.md) valeur qui spécifie si un fichier ou répertoire existant doit être ouvert ou un nouveau fichier ou répertoire doit être créé. Valeur par défaut est **adFailIfNotExists**. Si défini sur la valeur par défaut, le mode d’accès est obtenu à partir de la [Mode](../../../ado/reference/ado-api/mode-property-ado.md) propriété. Ce paramètre est ignoré lors de la *Source* paramètre ne contient pas une URL.  
  
 *Options*  
 Ce paramètre est facultatif. A [RecordOpenOptionsEnum](../../../ado/reference/ado-api/recordopenoptionsenum.md) valeur qui spécifie les options pour l’ouverture du **enregistrement**. Valeur par défaut est **adOpenRecordUnspecified**. Ces valeurs peuvent être combinées.  
  
 *UserName*  
 Ce paramètre est facultatif. A **chaîne** valeur qui contient l’ID utilisateur, si nécessaire, autorise l’accès aux *Source*.  
  
 *Mot de passe*  
 Ce paramètre est facultatif. A **chaîne** valeur qui contient le mot de passe, si nécessaire, vérifie *nom d’utilisateur*.  
  
## <a name="remarks"></a>Notes  
 *Source* peut être :  
  
-   UNE URL. Si le protocole de l’URL est http, le fournisseur Internet sera appelé par défaut. Si l’URL pointe vers un nœud qui contient un script exécutable (comme un. Page ASP), un **enregistrement** qui contient la source à la place exécuté contenu s’ouvre par défaut. Utilisez le *Options* argument pour modifier ce comportement.  
  
-   A **enregistrement** objet. A **enregistrement** objet ouvert à partir d’un autre **enregistrement** clone d’origine **enregistrement** objet.  
  
-   A **commande** objet. Le texte ouvert **enregistrement** objet représente la seule ligne retournée par l’exécution de la **commande**. Si les résultats contiennent plus d’une seule ligne, le contenu de la première ligne est placé dans l’enregistrement et une erreur peut être ajoutée à la **erreurs** collection.  
  
-   Une instruction SQL SELECT. Le texte ouvert **enregistrement** objet représente la seule ligne retournée par l’exécution du contenu de la chaîne. Si les résultats contiennent plus d’une seule ligne, le contenu de la première ligne est placé dans l’enregistrement et une erreur peut être ajoutée à la **erreurs** collection.  
  
-   Un nom de table.  
  
 Si le **enregistrement** objet représente une entité qui ne sont pas accessibles avec une URL (par exemple, une ligne d’un **Recordset** dérivé d’une base de données), les valeurs de la [ParentURL](../../../ado/reference/ado-api/parenturl-property-ado.md)propriété et le champ accédé avec la **adRecordURL** constante sont null.  
  
> [!NOTE]
>  URL à l’aide du modèle http appellent automatiquement le [fournisseur Microsoft OLE DB pour la publication Internet](../../../ado/guide/appendixes/microsoft-ole-db-provider-for-internet-publishing.md). Pour plus d’informations, consultez [URL absolues et relatives](../../../ado/guide/data/absolute-and-relative-urls.md).  
  
## <a name="applies-to"></a>S'applique à  
 [Record, objet (ADO)](../../../ado/reference/ado-api/record-object-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Open (méthode) (connexion ADO)](../../../ado/reference/ado-api/open-method-ado-connection.md)   
 [Open (méthode) (jeu d’enregistrements ADO)](../../../ado/reference/ado-api/open-method-ado-recordset.md)   
 [Open (méthode) (flux ADO)](../../../ado/reference/ado-api/open-method-ado-stream.md)   
 [OpenSchema, méthode](../../../ado/reference/ado-api/openschema-method.md)
