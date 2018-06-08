---
title: Gestion des assemblys de modèle multidimensionnel | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: df015e99df80915c68fa8f45e9f31ec475e22bc2
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="multidimensional-model-assemblies-management"></a>Gestion des assemblys de modèles multidimensionnels
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] offre de nombreuses fonctions intrinsèques à utiliser avec les langages MDX (Multidimensional Expressions) et les Extensions DMX (Data Mining), conçu pour effectuer toutes les opérations des calculs statistiques standard au parcours des membres dans une hiérarchie. Cependant, comme dans tout autre produit complexe et robuste, il est toujours nécessaire d'étendre la fonctionnalité de ce type d'outil.  
  
 Par conséquent, [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] vous permet d'ajouter des assemblys à une base de données ou à une instance d' [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] . Les assemblys vous permettent de créer des fonctions externes définies par l'utilisateur à l'aide de tout langage CLR (Common Language Runtime), tel que Microsoft Visual Basic .NET ou Microsoft Visual C#. Vous pouvez aussi utiliser des langages d'automatisation COM (Component Object Model) tels que Microsoft Visual Basic ou Microsoft Visual C++.  
  
> [!IMPORTANT]  
>  Les assemblys COM peuvent présenter un risque pour la sécurité. En raison de ce risque et d'autres considérations, les assemblys COM ont été déconseillés dans [!INCLUDE[ssASversion10](../../includes/ssasversion10-md.md)]. Les assemblys COM peuvent ne pas être pris en charge dans les versions ultérieures.  
  
 Les assemblys vous permettent d'étendre les fonctions d'entreprise de MDX et de DMX. Vous créez la fonctionnalité que vous souhaitez dans une bibliothèque de liens dynamiques (DLL), et vous ajoutez la bibliothèque en tant qu'assembly à une instance d' [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] ou à une base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] . Les méthodes publiques de la bibliothèque sont alors exposées en tant que fonctions définies par l'utilisateur aux expressions, procédures, calculs, actions et applications clientes MDX et DMX.  
  
 Un assembly avec les nouvelles procédures et les fonctions peut être ajouté au serveur. Vous pouvez utiliser des assemblys pour améliorer ou ajouter des fonctionnalités personnalisées qui ne sont pas fournies par le serveur. En utilisant des assemblys, vous pouvez ajouter de nouvelles fonctions aux expressions MDX (Multidimensional Expressions), aux Extensions DMX (Data Mining Extensions) ou aux procédures stockées. Les assemblys sont chargés à partir de l'emplacement où l'application personnalisée est exécutée, et une copie du fichier binaire d'assembly est enregistrée avec les données de base de données dans le serveur. Lorsqu'un assembly est supprimé, l'assembly copié est également supprimé du serveur.  
  
 Les assemblys peuvent être de deux types différents : COM et CLR. Les assemblys CLR sont de assemblys développés dans les langages de programmation du .NET Framework tels que C#, Visual Basic .NET, C++ managé. Les assemblys COM sont des bibliothèques COM qui doivent être enregistrées dans le serveur.  
  
 Les assemblys peuvent être ajoutés aux objets <xref:Microsoft.AnalysisServices.Server> ou <xref:Microsoft.AnalysisServices.Database> . Les assemblys de serveur peuvent être appelés par tout utilisateur connecté au serveur ou tout objet dans le serveur. Les assemblys de base de données peuvent être appelés uniquement par les objets <xref:Microsoft.AnalysisServices.Database> ou les utilisateurs connectés à la base de données.  
  
 Un objet <xref:Microsoft.AnalysisServices.Assembly> simple est composé d’informations de base (Nom et ID), d’une collection de fichiers et de caractéristiques de sécurité.  
  
 La collection de fichiers fait référence aux fichiers d'assembly chargés et aux fichiers de débogage (.pdb) correspondants, si les fichiers de débogage ont été chargés avec les fichiers d'assembly. Les fichiers d'assemblys sont chargés à partir de l'emplacement où l'application a défini les fichiers et une copie est enregistrée avec les données, dans le serveur. La copie du fichier d'assembly est utilisée pour charger l'assembly chaque fois que le service est démarré.  
  
 Les caractéristiques de sécurité incluent le jeu d'autorisations et l'emprunt d'identité utilisés pour exécuter l'assembly.  
  
## <a name="calling-user-defined-functions"></a>appel de fonctions définies par l’utilisateur  
 L'appel d'une fonction définie par l'utilisateur dans un assembly est semblable à l'appel d'une fonction intrinsèque, excepté que vous devez utiliser un nom complet. Par exemple, une fonction définie par l'utilisateur qui renvoie un type attendu par MDX est insérée comme ceci dans une requête MDX :  
  
```  
Select MyAssembly.MyClass.MyStoredProcedure(a, b, c) on 0 from Sales  
```  
  
 Les fonctions définies par l'utilisateur peuvent aussi être appelées à l'aide du mot clé CALL. Vous devez utiliser le mot clé CALL pour les fonctions définies par l'utilisateur qui renvoient des jeux d'enregistrements ou des valeurs de type void, et vous ne pouvez pas utiliser le mot clé CALL si la fonction définie par l'utilisateur dépend d'un objet dans le contexte de l'instruction ou du script MDX ou DMX, tel que le cube en cours ou le modèle d'exploration de données. Une utilisation courante d'une fonction appelée en dehors d'une requête MDX ou DMX est l'utilisation du modèle d'objets AMO pour effectuer des fonctions d'administration. Si vous souhaitez par exemple utiliser la fonction `MyVoidProcedure(a, b, c)` dans une instruction MDX, utilisez la syntaxe suivante :  
  
```  
Call MyAssembly.MyClass.MyVoidProcedure(a, b, c)  
```  
  
 Les assemblys simplifient le développement de bases de données en permettant à du code commun d'être développé une seule fois et d'être stocké à un seul emplacement. Les développeurs de logiciels clients peuvent créer des bibliothèques de fonctions pour [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] et les distribuer avec leurs applications.  
  
 Les assemblys et les fonctions définies par l'utilisateur peuvent dupliquer les noms des fonctions de la bibliothèque de fonctions de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] ou d'autres assemblys. Dès lors que vous appelez la fonction définie par l'utilisateur avec son nom complet, [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] utilise la procédure correcte. Pour des raisons de sécurité et pour éliminer la possibilité d'appeler un nom dupliqué dans une autre bibliothèque de classes, [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] requiert l'utilisation des seuls noms complets pour les procédures stockées.  
  
 Pour appeler une fonction définie par l'utilisateur depuis un assembly CLR spécifique, la fonction définie par l'utilisateur est précédée du nom de l'assembly, du nom complet de la classe et du nom de la procédure, comme dans l'exemple suivant :  
  
 *AssemblyName*.*FullClassName*.*ProcedureName*(*Argument1*, *Argument2*, ...)  
  
 Pour assurer la compatibilité amont avec les versions antérieures de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], la syntaxe suivante est également acceptable :  
  
 *AssemblyName*!*FullClassName*!*ProcedureName*(*Argument1*, *Argument2*, ...)  
  
 Si une bibliothèque COM prend en charge plusieurs interfaces, l'identificateur d'interface peut également être utilisé pour résoudre le nom de la procédure, comme dans cet exemple :  
  
 *AssemblyName*!*InterfaceID*!*ProcedureName*(*Argument1*, *Argument2*, ...)  
  
## <a name="security"></a>Sécurité  
 La sécurité des assemblys est basée sur le modèle de sécurité de .NET Framework, qui est un modèle de sécurité d'accès du code. .NET Framework prend en charge un mécanisme de sécurité d'accès du code qui suppose que le module d'exécution peut héberger à la fois du code d'un niveau de confiance total et d'un niveau de confiance partiel. Les ressources qui sont protégées par la sécurité d'accès du code de .NET Framework sont généralement intégrées à du code managé qui demande l'autorisation correspondante avant d'autoriser l'accès à la ressource. La demande d'autorisation est satisfaite seulement si tous les appelants (au niveau de l'assembly) de la pile d'appels ont l'autorisation pour la ressource correspondante.  
  
 Pour les assemblys, l’autorisation d’exécution est passée avec la propriété **PermissionSet** sur l’objet **Assembly** . Les autorisations reçues par le code managé sont déterminées par la stratégie de sécurité effective. Il existe déjà trois niveaux de stratégie effectifs dans un environnement hébergé non-[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] : entreprise, ordinateur et utilisateur. La liste effective des autorisations reçues par le code est déterminée par l'intersection des autorisations obtenues par ces trois niveaux.  
  
 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] fournit un niveau de stratégie de sécurité au niveau de l'hôte au CLR (Common Language Runtime) quand il l'héberge ; cette stratégie est un niveau de stratégie supplémentaire sous les trois niveaux de stratégie qui sont toujours effectifs. Cette stratégie est définie pour chaque domaine d'application qui est créé par [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].  
  
 La stratégie de niveau hôte de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] est une combinaison de la stratégie fixe de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] pour les assemblys système et de la stratégie spécifiée par l’utilisateur pour les assemblys utilisateur. La partie spécifiée par l'utilisateur de la stratégie d'hôte d' [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] est basée sur le propriétaire de l'assembly spécifiant un des trois compartiments d'autorisation pour chaque assembly :  
  
|Paramètre d'autorisation| Description|  
|------------------------|-----------------|  
|**Sécurisé**|Fournit une autorisation de traitement interne. Ce compartiment d'autorisations ne donne pas d'autorisations pour accéder aux ressources protégées dans le .NET Framework. Il s’agit du compartiment d’autorisations utilisé par défaut pour un assembly si aucun n’est spécifié avec la propriété **PermissionSet** .|  
|**ExternalAccess**|Fournit le même accès que le paramètre **Safe** , avec la possibilité supplémentaire de pouvoir accéder à des ressources système externes. Ce compartiment d'autorisations n'offre pas de garanties de sécurité (même s'il est possible de sécuriser ce scénario), mais il donne des garanties de fiabilité.|  
|**Non sécurisé**|Ne fournit pas de restrictions. Aucune garantie de sécurité ou de fiabilité ne peut être donnée pour du code managé s'exécutant sous cet ensemble d'autorisations. Toutes les autorisations, même une autorisation personnalisée incluse par l'administrateur, sont accordées au code s'exécutant à ce niveau de confiance.|  
  
 Quand le CLR est hébergé par [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], la vérification des autorisations basée sur le parcours de pile s'arrête à la limite avec le code [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] natif. Tout code managé dans des assemblys [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] tombe toujours dans une des trois catégories d'autorisations dont la liste est donnée ci-dessus.  
  
 Les routines d'assembly COM (ou non managée) ne prennent pas en charge le modèle de sécurité CLR.  
  
### <a name="impersonation"></a>Emprunt d'identité  
 Quand du code managé accède à une ressource en dehors d’ [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] suit les règles associées à un paramètre de la propriété **ImpersonationMode** de l’assembly de façon à garantir que l’accès se fait dans un contexte de sécurité Windows approprié. Étant donné que les assemblys qui utilisent le paramètre d’autorisation **Safe** ne peuvent pas accéder à des ressources en dehors d’ [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], ces règles sont applicables uniquement pour les assemblys utilisant les paramètres d’autorisation **ExternalAccess** et **Unsafe** .  
  
-   Si le contexte d’exécution actuel correspond à une connexion authentifiée Windows et qu’il est le même que le contexte de l’appelant d’origine (c’est-à-dire qu’il n’y a pas d’instruction intermédiaire EXECUTE AS), [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] emprunte l’identité de la connexion authentifiée Windows avant d’accéder à la ressource.  
  
-   S'il y a une instruction EXECUTE AS intermédiaire qui a changé le contexte relativement à celui de l'appelant d'origine, la tentative d'accès à une ressource externe échoue.  
  
 La propriété **ImpersonationMode** peut être définie avec le paramètre **ImpersonateCurrentUser** ou **ImpersonateAnonymous**. Le paramètre par défaut, **ImpersonateCurrentUser**, exécute un assembly sous le compte de connexion réseau de l’utilisateur actuel. Si le paramètre **ImpersonateAnonymous** est utilisé, le contexte d’exécution correspond au compte d’utilisateur de connexion Windows IUSER_*nom_serveur* sur le serveur. Il s'agit du compte Invité Internet, qui a des droits limités sur le serveur. Un assembly s'exécutant dans ce contexte peut seulement accéder à des ressources limitées sur le serveur local.  
  
### <a name="application-domains"></a>Domaines d'application  
 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] n’expose pas directement les domaines d’application. Grâce à un ensemble d’assemblys s’exécutant dans le même domaine d’application, les domaines d’application sont capables de se découvrir les uns les autres au moment de l’exécution à l’aide de l’espace de noms **System.Reflection** dans .NET Framework ou par d’autres moyens. Ils sont également capables d’y faire des appels à liaison tardive. De tels appels font l'objet des vérifications d'autorisations utilisées par la sécurité basée sur les autorisations de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] .  
  
 Il est recommandé de ne pas se baser sur la recherche d'assemblys dans le même domaine d'application, dans la mesure où la limite du domaine d'application et les assemblys qui vont dans chacun des domaines sont définis par la mise en œuvre.  
  
## <a name="see-also"></a>Voir aussi  
 [Définition de la sécurité pour les procédures stockées](../../analysis-services/multidimensional-models-extending-olap-stored-procedures/setting-security-for-stored-procedures.md)   
 [Définition de procédures stockées](../../analysis-services/multidimensional-models-extending-olap-stored-procedures/defining-stored-procedures.md)  
  
  
