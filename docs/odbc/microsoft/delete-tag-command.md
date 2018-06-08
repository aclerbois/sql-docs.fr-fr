---
title: Commande de balise DELETE | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- DELETE TAG command [ODBC]
ms.assetid: 4f4e1362-a5f3-4b15-8a3c-d4e96605f221
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 28ee28e069ac0e1ef8e22ca2b118e273236e269c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="delete-tag-command"></a>SUPPRIMER des commandes de balise
Supprime une ou plusieurs balises à partir d’un fichier d’index composés (.cdx).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
DELETE TAG TagName1 [OF CDXFileName1]  
   [, TagName2 [OF CDXFileName2]] ...  
  Or   
DELETE TAG ALL [OF CDXFileName]  
```  
  
## <a name="arguments"></a>Arguments  
 *TagName1*OF *CDXFileName1*[, *TagName2*[OF *CDXFileName2*]]...  
 Spécifie un marqueur à supprimer à partir d’un fichier d’index composés. Vous pouvez supprimer plusieurs balises avec une balise à supprimer en incluant une liste de noms de balises séparées par des virgules. Si deux ou plusieurs étiquettes portant le même nom existent dans les fichiers d’index ouvert, vous pouvez supprimer une balise à partir d’un fichier d’index spécifique en incluant de *CDXFileName*.  
  
 Tous les [OF *CDXFileName*]  
 Supprime toutes les balises à partir d’un fichier d’index composés. Si la table actuelle comporte un fichier d’index composés structurelle, toutes les balises sont supprimées du fichier d’index, le fichier d’index est supprimé à partir du disque et l’indicateur dans l’en-tête du tableau indiquant la présence d’un fichier d’index composés structurelle associé est supprimé. Utilisez toutes les données avec de *CDXFileName* pour supprimer toutes les balises d’un fichier ouvert index composé autre que le fichier d’index composés structurel.  
  
## <a name="remarks"></a>Notes  
 Les fichiers d’index composés, créés avec un INDEX, contient des balises correspondant aux entrées d’index. SUPPRIMER la balise est utilisée pour supprimer une ou plusieurs balises à partir des fichiers d’index composé ouvert. Vous pouvez supprimer uniquement les balises à partir des fichiers d’index composé ouverts dans la zone de travail en cours. Si vous supprimez toutes les balises à partir d’un fichier d’index composés, le fichier est supprimé à partir du disque.  
  
 Visual FoxPro recherche d’abord dans une balise dans le fichier d’index composés structurelle (si un est ouvert). Si la balise n’est pas dans le fichier d’index composés structurelle, Visual FoxPro recherche ensuite la balise dans les autres fichiers d’index composé ouvert.  
  
## <a name="see-also"></a>Voir aussi  
 [INDEX, commande](../../odbc/microsoft/index-command.md)
