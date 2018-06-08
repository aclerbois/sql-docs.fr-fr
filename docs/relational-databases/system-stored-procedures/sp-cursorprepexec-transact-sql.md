---
title: sp_cursorprepexec (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-stored-procedures
ms.reviewer: ''
ms.suite: sql
ms.technology: system-objects
ms.tgt_pltfrm: ''
ms.topic: language-reference
f1_keywords:
- sp_cursorprepexec_TSQL
- sp_cursorprepexec
dev_langs:
- TSQL
helpviewer_keywords:
- sp_cursorprepexec
ms.assetid: 8094fa90-35b5-4cf4-8012-0570cb2ba1e6
caps.latest.revision: 9
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 378a389780e8af6ed966c4e0757352b16fbc0dd1
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="spcursorprepexec-transact-sql"></a>sp_cursorprepexec (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Compile un plan pour le lot ou l'instruction de curseur envoyé, puis crée et remplit le curseur. sp_cursorprepexec combine les fonctions de sp_cursorprepare et sp_cursorexecute. Cette procédure est appelée en spécifiant ID = 5 dans un paquet TDS (Tabular Data Stream).  
  
 ![Icône de lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_cursorprepexec prepared handle OUTPUT, cursor OUTPUT, params , statement , options  
    [ , scrollopt [ , ccopt [ , rowcount ] ] ]  
```  
  
## <a name="arguments"></a>Arguments  
 *handle préparé*  
 Est un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] généré préparé *gérer* identificateur. *handle préparé* est obligatoire et renvoie **int**.  
  
 *cursor*  
 Identificateur de curseur généré par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. *curseur* est un paramètre obligatoire qui doit être fourni pour toutes les procédures suivantes qui agissent sur ce curseur, par exemple sp_cursorfetch.  
  
 *Params*  
 Identifie des instructions paramétrables. Le *params* définition de variables est remplacée par des marqueurs de paramètre dans l’instruction. *params* est un paramètre obligatoire qui demande un **ntext**, **nchar**, ou **nvarchar** valeur d’entrée.  
  
> [!NOTE]  
>  Utilisez un **ntext** chaîne en tant que l’entrée valeur lorsque *stmt* est paramétré et *scrollopt* parameterized_stmt est ON.  
  
 *instruction*  
 Définit le jeu de résultats de curseur. Le *instruction* paramètre est obligatoire et demande pour un **ntext**, **nchar** ou **nvarchar** valeur d’entrée.  
  
> [!NOTE]  
>  Les règles de spécification de la valeur stmt sont les mêmes que celles pour sp_cursoropen, sauf que la *stmt* type de données de chaîne doit être **ntext**.  
  
 *options*  
 Paramètre optionnel qui retourne une description des colonnes du jeu de résultats du curseur. *options* requiert les éléments suivants **int** valeur d’entrée.  
  
|Valeur| Description|  
|-----------|-----------------|  
|0x0001|RETURN_METADATA|  
  
 *paramètres scrollopt*  
 Option de défilement. *paramètres scrollopt* est un paramètre optionnel qui requiert l’une des opérations suivantes **int** valeurs d’entrée.  
  
|Valeur| Description|  
|-----------|-----------------|  
|0x0001|KEYSET|  
|0x0002|DYNAMIC|  
|0x0004|FORWARD_ONLY|  
|0x0008|STATIC|  
|0x10|FAST_FORWARD|  
|0x1000|PARAMETERIZED_STMT|  
|0x2000|AUTO_FETCH|  
|0x4000|AUTO_CLOSE|  
|0x8000|CHECK_ACCEPTED_TYPES|  
|0x10000|KEYSET_ACCEPTABLE|  
|0x20000|DYNAMIC_ACCEPTABLE|  
|0x40000|FORWARD_ONLY_ACCEPTABLE|  
|0x80000|STATIC_ACCEPTABLE|  
|0x100000|FAST_FORWARD_ACCEPTABLE|  
  
 En raison de la possibilité que l’option demandée n’est pas appropriée pour le curseur défini par  *\<stmt >*, ce paramètre sert à la fois d’entrée et sortie. Dans de tels cas, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] affecte un type approprié et modifie cette valeur.  
  
 *ccopt*  
 Option de contrôle en matière d'accès concurrentiel. *ccopt* est un paramètre optionnel qui requiert l’une des opérations suivantes **int** valeurs d’entrée.  
  
|Valeur| Description|  
|-----------|-----------------|  
|0x0001|READ_ONLY|  
|0x0002|SCROLL_LOCKS (précédemment appelé LOCKCC)|  
|0x0004|**OPTIMISTE** (précédemment appelé optcc)|  
|0x0008|OPTIMISTIC (précédemment appelé OPTCCVAL)|  
|0x2000|ALLOW_DIRECT|  
|0x4000|UPDT_IN_PLACE|  
|0x8000|CHECK_ACCEPTED_OPTS|  
|0x10000|READ_ONLY_ACCEPTABLE|  
|0x20000|SCROLL_LOCKS_ACCEPTABLE|  
|0x40000|OPTIMISTIC_ACCEPTABLE|  
|0x80000|OPTIMISITC_ACCEPTABLE|  
  
 Comme avec *scrollpt*, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut affecter une valeur différente de celle demandée.  
  
 *nombre de lignes*  
 Paramètre optionnel qui indique le nombre de lignes de tampon d'extraction à utiliser avec AUTO_FETCH. La valeur par défaut est de 20 lignes. *nombre de lignes* se comporte différemment lorsque affecté comme une valeur d’entrée ou une valeur de retour.  
  
|Comme une valeur d'entrée|Comme une valeur de retour|  
|--------------------|---------------------|  
|Quand AUTO_FETCH est spécifiée avec les curseurs FAST_FORWARD *rowcount* représente le nombre de lignes à placer dans le tampon d’extraction.|Représente le nombre de lignes dans le jeu de résultats. Lorsque le *scrollopt* AUTO_FETCH valeur est spécifiée, *rowcount* retourne le nombre de lignes qui ont été extraites dans le tampon d’extraction.|  
  
## <a name="return-code-values"></a>Valeurs des codes de retour  
 Si *params* retourne une valeur NULL, puis l’instruction n’est pas paramétrable.  
  
## <a name="see-also"></a>Voir aussi  
 [sp_cursoropen &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-cursoropen-transact-sql.md)   
 [sp_cursorexecute &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-cursorexecute-transact-sql.md)   
 [sp_cursorprepare &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-cursorprepare-transact-sql.md)   
 [sp_cursorfetch &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-cursorfetch-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
