---
title: Spécifier une action de point d’arrêt | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.technology: scripting
ms.reviewer: ''
ms.suite: sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- vs.debug.breakpt.action
helpviewer_keywords:
- Transact-SQL debugger, breakpoint action
- Transact-SQL debugger, breakpoint when hit action
ms.assetid: f97f0097-6f51-40c1-b2e0-294a93ce1e1b
caps.latest.revision: 10
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: bba57b4969ea78e22a218cc9c3048684628fdfeb
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="specify-a-breakpoint-action"></a>Spécifier une action de point d'arrêt
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
  L’action de point d’arrêt **Lorsqu’il est atteint** spécifie une tâche personnalisée que le débogueur [!INCLUDE[tsql](../../includes/tsql-md.md)] effectue pour un point d’arrêt. Si le nombre d'accès spécifié est atteint et si les conditions de point d'arrêt spécifiées sont satisfaites, le débogueur effectue l'action spécifiée pour le point d'arrêt.  
  
##  <a name="BKMK_ActionConsiderations"></a> Considérations sur l'action  
 L'action par défaut pour un point d'arrêt consiste à arrêter l'exécution lorsque le nombre d'accès et la condition de point d'arrêt sont tous les deux satisfaits. L’action **Lorsqu’il est atteint** dans le débogueur [!INCLUDE[tsql](../../includes/tsql-md.md)] sert, quant à elle, principalement à afficher des informations dans la fenêtre **Sortie** du débogueur en spécifiant un message à afficher.  
  
 Le message à afficher est indiqué dans l’option **Afficher un message** et spécifié comme une chaîne de texte qui inclut des expressions contenant des informations issues du [!INCLUDE[tsql](../../includes/tsql-md.md)] débogué. Ces expressions sont notamment les suivantes.  
  
-   Une expression [!INCLUDE[tsql](../../includes/tsql-md.md)] comprise entre des accolades ({}). Les expressions peuvent inclure des variables [!INCLUDE[tsql](../../includes/tsql-md.md)] , des paramètres et des fonctions intégrées, par exemple {@MyVariable}, {@NameParameter}, {@@SPID} ou {SERVERPROPERTY(‘ProcessID’)}.  
  
-   Un des mots clés suivants :  
  
    1.  $ADDRESS retourne le nom de la procédure stockée ou de la fonction définie par l'utilisateur dans laquelle le point d'arrêt est défini. Si le point d'arrêt est défini dans la fenêtre de l'éditeur, $ADDRESS retourne le nom du fichier de script modifié. $ADDRESS et $FUNCTION retournent les mêmes informations dans le débogueur [!INCLUDE[tsql](../../includes/tsql-md.md)] .  
  
    2.  $CALLER retourne le nom de l'unité de code [!INCLUDE[tsql](../../includes/tsql-md.md)] qui a appelé une procédure stockée ou une fonction. Si le point d’arrêt se trouve dans la fenêtre de l’éditeur, $CALLER retourne \<Aucun appelant disponible>. Si le point d'arrêt se trouve dans une procédure stockée ou une fonction définie par l'utilisateur appelée à partir du code dans la fenêtre de l'éditeur, $CALLER retourne le nom du fichier modifié. Si le point d'arrêt se trouve dans une procédure stockée ou une fonction définie par l'utilisateur appelée à partir d'une autre procédure stockée ou fonction, $CALLER retourne le nom de la procédure ou fonction appelante.  
  
    3.  $CALLSTACK retourne la pile des appels des fonctions dans la chaîne qui a appelé la procédure stockée ou la fonction définie par l'utilisateur en cours. Si le point d'arrêt se trouve dans la fenêtre de l'éditeur, $CALLSTACK retourne le nom du fichier de script modifié.  
  
    4.  $FUNCTION retourne le nom de la procédure stockée ou de la fonction définie par l'utilisateur dans laquelle le point d'arrêt est défini. Si le point d'arrêt est défini dans la fenêtre de l'éditeur, $FUNCTION retourne le nom du fichier de script modifié.  
  
    5.  $PID et $PNAME retournent l'ID et le nom du processus de système d'exploitation qui exécute l'instance du moteur de base de données où le code [!INCLUDE[tsql](../../includes/tsql-md.md)] s'exécute. $PID retourne le même ID que SERVERPROPERTY(‘ProcessID’), mais $PID est une valeur hexadécimale alors que SERVERPROPERTY(‘ProcessID’) est une valeur décimale.  
  
    6.  $TID et $TNAME retournent l'ID et le nom du thread de système d'exploitation qui exécute le lot [!INCLUDE[tsql](../../includes/tsql-md.md)] . Le thread est associé au processus qui exécute l'instance du moteur de base de données. $TID retourne la même valeur que SELECT kpid FROM sys.sysprocesses WHERE spid = @@SPID, mais $TID est une valeur hexadécimale alors que kpid est une valeur décimale.  
  
-   Vous pouvez également utiliser la barre oblique inverse (\\) comme caractère d’échappement pour autoriser des accolades et des barres obliques inverses dans le message : \\{, \\} et \\\\.  
  
#### <a name="to-specify-a-when-hit-action"></a>Pour spécifier une action Lorsqu'il est atteint  
  
1.  Dans la fenêtre de l’éditeur, cliquez avec le bouton droit sur le glyphe du point d’arrêt, puis cliquez sur **Lorsqu’il est atteint** dans le menu contextuel.  
  
     -ou-  
  
     Dans la fenêtre des **points d’arrêt** , cliquez avec le bouton droit sur le glyphe du point d’arrêt, puis cliquez sur **Lorsqu’il est atteint** dans le menu contextuel.  
  
2.  Dans la boîte de dialogue **Lorsque le point d’arrêt est atteint** , sélectionnez le comportement que vous souhaitez :  
  
    1.  Sélectionnez **Afficher un message** pour afficher un message dans la fenêtre Sortie du débogueur lorsque le point d’arrêt est atteint.  
  
    2.  L’option **Exécuter une macro** n’est pas disponible à partir du débogueur [!INCLUDE[tsql](../../includes/tsql-md.md)] et est grisée.  
  
    3.  Sélectionnez **Continuer l’exécution** si vous ne voulez pas que le point d’arrêt suspende l’exécution. Cette option est active uniquement si vous avez sélectionné l’option **Afficher un message** .  
  
3.  Cliquez sur **OK** pour implémenter les modifications ou sur **Annuler** pour fermer sans appliquer les modifications.  
  
## <a name="see-also"></a> Voir aussi  
 [Spécifier une condition de point d'arrêt](../../relational-databases/scripting/specify-a-breakpoint-condition.md)   
 [Spécifier un nombre d’accès](../../relational-databases/scripting/specify-a-hit-count.md)  
  
  
