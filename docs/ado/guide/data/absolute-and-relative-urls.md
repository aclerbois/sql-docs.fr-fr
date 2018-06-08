---
title: URL absolues et relatives | Documents Microsoft
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
helpviewer_keywords:
- relative URLs [ADO]
- absolute URLs [ADO]
- URLs [ADO]
ms.assetid: 6a34a7ef-50cc-4c3d-82f7-106b9a8f3caf
caps.latest.revision: 12
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: ba1ab2c9bdc1adbc063cd05e88cf9d0001efb15c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="absolute-and-relative-urls"></a>URL absolues et relatives
Une URL spécifie l’emplacement d’une cible sur un ordinateur local ou en réseau. La cible peut être un fichier, répertoire, page HTML, image, programme et ainsi de suite *.*  
  
 Un *URL absolue* contient toutes les informations nécessaires pour localiser une ressource.  
  
 A *URL relative* localise une ressource à l’aide d’une URL absolue comme point de départ. En effet, le « URL complète » de la cible est spécifié en concaténant les URL absolues et relatives.  
  
 Un *URL absolue* utilise le format suivant : *scheme://server/path/resource*  
  
 Une URL relative comprend généralement uniquement le *chemin d’accès*et, éventuellement, le *ressource*, mais aucune *schéma* ou *server*. Les tableaux suivants décrivent les différentes parties du format d’URL complète.  
  
 *scheme*  
 Spécifie comment la *ressource* est accessible.  
  
 *server*  
 Spécifie le nom de l’ordinateur sur lequel le *ressource* se trouve.  
  
 *path*  
 Spécifie la séquence de répertoires menant à la cible. Si *ressource* est omis, la cible est le dernier répertoire dans *chemin d’accès*.  
  
 *resource*  
 Si inclus, *ressource* est la cible, et est généralement le nom d’un fichier. Il peut être un *fichier simple,* contenant un flux binaire d’octets, ou un *document structuré,* contenant un ou plusieurs stockages et des flux binaires d’octets.  
  
## <a name="url-scheme-registration"></a>Enregistrement du modèle d’URL  
 Si un fournisseur prend en charge les URL, le fournisseur s’enregistre un ou plusieurs schémas d’URL. L’inscription signifie que les URL utilisant le schéma appellent automatiquement le fournisseur enregistré. Par exemple, le *http* modèle est enregistré pour le [fournisseur Microsoft OLE DB pour la publication Internet](../../../ado/guide/appendixes/microsoft-ole-db-provider-for-internet-publishing.md). ADO suppose que toutes les URL sont précédés de « http » représentent des dossiers ou fichiers à utiliser avec le fournisseur de publication Web. Pour plus d’informations sur les modèles enregistrés par votre fournisseur, consultez la documentation du fournisseur.  
  
## <a name="defining-context-with-a-url"></a>Définissez le contexte avec une URL  
 Une fonction d’une connexion ouverte, représentée par un [connexion](../../../ado/reference/ado-api/connection-object-ado.md) de l’objet, est de limiter les opérations ultérieures à la source de données représentée par cette connexion. Autrement dit, la connexion définit le contexte pour les opérations suivantes.  
  
 Avec ADO 2.7 ou version ultérieure, une URL absolue peut également définir un contexte. Par exemple, lorsqu’un [enregistrement](../../../ado/reference/ado-api/record-object-ado.md) objet est ouvert avec une URL absolue, un **connexion** objet est implicitement créé pour représenter la ressource spécifiée par l’URL.  
  
 Une URL absolue qui définit un contexte peut être spécifiée dans le *ActiveConnection* paramètre de la **enregistrement** objet [ouvrir](../../../ado/reference/ado-api/open-method-ado-record.md) (méthode). Une URL absolue peut également être spécifiée comme la valeur de la « URL**=**« mot clé dans le **connexion** objet [ouvrir](../../../ado/reference/ado-api/open-method-ado-connection.md) (méthode)  *ConnectionString* paramètre et le [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) objet [ouvrir](../../../ado/reference/ado-api/open-method-ado-recordset.md) méthode *ActiveConnection* paramètre.  
  
 Le contexte peut également être défini en ouvrant un **enregistrement** ou **Recordset** objet qui représente un répertoire, car ces objets ont déjà un implicitement ou explicitement déclaré **connexion**  objet qui spécifie le contexte.  
  
## <a name="scoped-operations"></a>Opérations étendues  
 Le contexte définit également la portée, autrement dit, le répertoire et ses sous-répertoires qui peuvent participer aux opérations suivantes. Le **enregistrement** objet a plusieurs méthodes étendues qui ne fonctionne pas sur un répertoire et tous ses sous-répertoires. Ces méthodes incluent [CopyRecord](../../../ado/reference/ado-api/copyrecord-method-ado.md), [MoveRecord](../../../ado/reference/ado-api/moverecord-method-ado.md), et [DeleteRecord](../../../ado/reference/ado-api/deleterecord-method-ado.md).  
  
## <a name="relative-urls-as-command-text"></a>URL relatives sous forme de texte de commande  
 Vous pouvez spécifier une commande à exécuter sur la source de données en tapant une chaîne dans le *CommandText* paramètre de la **connexion** l’objet [Execute](../../../ado/reference/ado-api/execute-method-ado-connection.md) (méthode) et dans le  *Source* paramètre de la **Recordset** l’objet [ouvrir](../../../ado/reference/ado-api/open-method-ado-recordset.md) (méthode).  
  
 Une URL relative peut être spécifiée dans le *CommandText* ou *Source* paramètre. L’URL relative ne représente pas une commande, telle qu’une commande SQL ; Elle indique simplement les paramètres. Le contexte de la connexion active doit être une URL absolue et le *Option* paramètre doit être défini sur **adCmdTableDirect**.  
  
 Par exemple, l’exemple de code suivant montre comment ouvrir un **Recordset** sur le fichier Readme25.txt du répertoire Winnt/system32 :  
  
```  
recordset.Open "system32/Readme25.txt", "URL=http://YourServer/Winnt/",,,adCmdTableDirect  
```  
  
 L’URL absolue dans la chaîne de connexion spécifie le serveur (`YourServer`) et le chemin d’accès (`Winnt`). Cette URL définit également le contexte.  
  
 L’URL relative dans le texte de commande utilise l’URL absolue comme point de départ et spécifie le reste du chemin d’accès (`system32`) et le fichier à ouvrir (`Readme25.txt`).  
  
 Le champ d’options (`adCmdTableDirect`) indique que le type de commande est une URL relative.  
  
 Autre exemple, le code suivant ouvre un **Recordset** sur le contenu de la `Winnt` active :  
  
```  
recordset.Open "", "URL=http://YourServer/Winnt/",,,adCmdTableDirect  
```  
  
## <a name="ole-db-provider-supplied-url-schemes"></a>Schémas d’URL fournis par le fournisseur OLE DB  
 Le début d’une URL complète est la *schéma* qui est utilisé pour accéder à la ressource identifiée par le reste de l’URL. HTTP (Hypertext Transfer Protocol) et FTP (File Transfer Protocol) sont des exemples.  
  
 ADO prend en charge les fournisseurs OLE DB qui reconnaissent leurs propres modèles d’URL. Par exemple, le [fournisseur Microsoft OLE DB pour la publication Internet](../../../ado/guide/appendixes/microsoft-ole-db-provider-for-internet-publishing.md)*,* qui accède aux fichiers de Windows 2000 « publiés », reconnaît le modèle HTTP existant.  
  
## <a name="see-also"></a>Voir aussi  
 [Objet de connexion (ADO)](../../../ado/reference/ado-api/connection-object-ado.md)   
 [Objet d’enregistrement (ADO)](../../../ado/reference/ado-api/record-object-ado.md)   
 [Recordset, objet (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md)
