---
title: Débogage des procédures stockées | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: olap
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: d37eebfad3f8a3e89dc65ad9602f4b5d61b5072e
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="debugging-stored-procedures"></a>Débogage des procédures stockées
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Les procédures stockées [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] sont en réalité des bibliothèques CLR ou COM (normalement des DLL) écrites en C# (ou dans tout autre langage CLR ou COM). Par conséquent, le débogage d'une procédure stockée n'est pas sans ressembler à celui des autres applications dans l'environnement de débogage de Visual Studio. Pour déboguer les procédures stockées, l'environnement de développement de Visual Studio met à votre disposition ses fonctions de débogage intégrées. Ces fonctions vous permettent d'arrêter l'exécution aux emplacements des procédures, d'inspecter des valeurs en mémoire et dans le Registre, de modifier des variables, d'observer les échanges de messages et de regarder de près comment votre code fonctionne.  
  
### <a name="to-debug-a-stored-procedure"></a>Pour déboguer une procédure stockée  
  
1.  Ouvrez le projet utilisé pour créer la DLL dans Visual Studio.  
  
2.  Créez des points d'arrêt dans la méthode ou la fonction correspondant à la procédure à déboguer.  
  
3.  Utilisez Visual Studio pour créer une version debug d'une DLL de procédure stockée.  
  
4.  Déployez la DLL vers le serveur. Pour plus d’informations sur le déploiement de la DLL vers le serveur, consultez [création de procédures stockées](../../analysis-services/multidimensional-models-extending-olap-stored-procedures/creating-stored-procedures.md).  
  
5.  Vous avez besoin d'une application qui appelle la procédure stockée que vous voulez tester. Si vous n'avez pas d'application prête à jouer ce rôle, vous pouvez utiliser l'éditeur de requête MDX dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] pour créer une requête MDX qui appelle la procédure stockée à tester.  
  
6.  Dans Visual Studio, attachez la DLL au processus [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] (Msmdsrv.exe).  
  
    1.  À partir de la **déboguer** menu, choisissez **attacher toProcess**.  
  
    2.  Dans le **attacher toProcess** boîte de dialogue, sélectionnez **afficher les processus de tous les utilisateurs**.  
  
    3.  Dans le **processus disponibles** liste, dans le **processus** colonne, cliquez sur **Msmdsrv.exe**. S'il y a plus d'une instance de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] qui s'exécute sur le serveur, vous devez identifier le processus par l'identificateur de l'instance que vous souhaitez utiliser.  
  
    4.  Dans le **attacher à** texte zone, assurez-vous que le type de programme approprié est sélectionné. Pour une DLL CLR, cliquez sur **sélectionnez**, puis cliquez sur **déboguer ces types de codes**, puis cliquez sur **Managed**, puis cliquez sur **OK**. Pour une DLL COM, cliquez sur **sélectionnez**, puis cliquez sur **déboguer ces types de codes**, puis cliquez sur **natif**, puis cliquez sur **OK**.  
  
    5.  Cliquez sur **attacher**.  
  
7.  Dans [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], appelez le programme ou le script MDX qui appelle la procédure stockée. Le débogueur s'interrompt lorsqu'il atteint une ligne contenant un point d'arrêt. Vous pouvez évaluer des variables dans la fenêtre Espion, afficher des variables locales et exécuter le code en pas à pas.  
  
 Si le débogage d'une bibliothèque pose des problèmes, vérifiez que le fichier de base de données du programme (fichier PDB) correspondant a été copié vers l'emplacement du déploiement sur le serveur. Si ce fichier n'a pas été copié durant l'inscription ou le déploiement, vous devez le copier manuellement au même endroit que la DLL. Pour le code natif (DLL COM), le fichier PDB réside dans le sous-répertoire \debug. Pour le code managé (DLL CLR), il réside dans le sous-répertoire \WINDEBUG.  
  
## <a name="see-also"></a>Voir aussi  
 [Gestion des assemblys de modèle multidimensionnel](../../analysis-services/multidimensional-models/multidimensional-model-assemblies-management.md)   
 [Définition de procédures stockées](../../analysis-services/multidimensional-models-extending-olap-stored-procedures/defining-stored-procedures.md)  
  
  
