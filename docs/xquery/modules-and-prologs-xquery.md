---
title: Modules et prologues (XQuery) | Documents Microsoft
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql
ms.component: xquery
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: language-reference
applies_to:
- SQL Server
dev_langs:
- XML
helpviewer_keywords:
- XQuery, prolog
- prolog
ms.assetid: 0f17b4a4-6234-41d4-a996-6db4e27bff7e
caps.latest.revision: 13
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: 08d19f2a24c243647b4004a557fd25bf96d2dd62
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="modules-and-prologs-xquery"></a>Modules et prologues (XQuery)
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-xxx-md](../includes/tsql-appliesto-ss2012-xxxx-xxxx-xxx-md.md)]

  [Prologue XQuery](../xquery/modules-and-prologs-xquery-prolog.md) est une série de déclarations d’espace de noms. En utilisant la déclaration d'espace de noms en prologue, vous pouvez spécifier une liaison entre un préfixe et un espace de noms, puis utiliser le préfixe dans le corps de la requête.  
  
## <a name="implementation-limitations"></a>Limites de mise en œuvre  
 Les spécifications XQuery suivantes ne sont pas prises en charge dans cette implémentation :  
  
-   Déclaration de module (`version`)  
  
-   Déclaration de module (`module namespace`)  
  
-   Xmpspacedeclaration (`xmlspace`)  
  
-   Déclaration de classement par défaut (`declare default collation`)  
  
-   Déclaration d'URI de base (`declare base-uri`)  
  
-   Déclaration de construction (`declare construction`)  
  
-   Déclaration d'ordre par défaut (`declare ordering`)  
  
-   Importation de schéma (`import schema namespace`)  
  
-   Importation de module (`import module`)  
  
-   Déclaration de variable dans le prologue (`declare variable`)  
  
-   Déclaration de fonction (`declare function`)  
  
## <a name="in-this-section"></a>Dans cette section  
 [Prologue XQuery](../xquery/modules-and-prologs-xquery-prolog.md)  
 Décrit le prologue XQuery.  
  
## <a name="see-also"></a>Voir aussi  
 [Références relatives au langage Xquery &#40;SQL Server&#41;](../xquery/xquery-language-reference-sql-server.md)  
  
  
