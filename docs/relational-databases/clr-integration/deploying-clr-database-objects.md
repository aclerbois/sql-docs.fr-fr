---
title: Déploiement d’objets de base de données CLR | Documents Microsoft
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: clr
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- deployment script [CLR integration]
- common language runtime [SQL Server], deploying
- deploying assemblies [CLR integration]
- deploying [CLR integration]
ms.assetid: 00752573-3367-41a7-af98-7b7a29e8e2f2
caps.latest.revision: 35
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: 353aef3aac9f39217bb98bd551843eb1b69ae885
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="deploying-clr-database-objects"></a>Déploiement d'objets de base de données CLR
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Le déploiement est le processus selon lequel une application ou un module fini est distribué en vue de son installation et de son exécution sur un autre ordinateur. [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual Studio vous permet de développer des objets de base de données CLR (Common Language Runtime) et de les déployer sur un serveur de test. Les objets de base de données managés peuvent également être compilés avec les fichiers de redistribution [!INCLUDE[msCoName](../../includes/msconame-md.md)] .NET Framework, au lieu de Visual Studio. Une fois compilés, les assemblys qui contiennent les objets de base de données CLR peuvent être déployés sur un serveur de test à l'aide de Visual Studio ou d'instructions [!INCLUDE[tsql](../../includes/tsql-md.md)]. Notez que Visual Studio .NET 2003 ne peut pas être utilisé pour le déploiement ou la programmation de l'intégration du CLR. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] inclut le .NET Framework préinstallé et Visual Studio .NET 2003 ne peut pas utiliser les assemblys .NET Framework 2.0.  
  
 Une fois que les méthodes CLR ont été testées et vérifiées sur le serveur de test, elles peuvent être distribuées sur les serveurs de production à l'aide d'un script de déploiement. Le script de déploiement peut être généré manuellement ou à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] (consultez la procédure plus loin dans cette rubrique).  
  
 La fonctionnalité d'intégration du CLR est désactivée par défaut dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et doit être activée pour utiliser des assemblys CLR. Pour plus d’informations, consultez [Activation de l’intégration du CLR](../../relational-databases/clr-integration/clr-integration-enabling.md).  
  
## <a name="deploying-the-assembly-to-the-test-server"></a>Déploiement de l'assembly sur le serveur de test  
 À l'aide de Visual Studio, vous pouvez développer des fonctions, procédures, déclencheurs, types définis par l'utilisateur (UDT) ou agrégats définis par l'utilisateur (UDA) CLR et les déployer sur un serveur de test. Ces objets de base de données managés peuvent également être compilés avec les compilateurs de ligne de commande, tels que csc.exe et vbc.exe, inclus avec les fichiers de redistribution .NET Framework. L'environnement de développement intégré Visual Studio n'est pas nécessaire pour développer des objets de base de données managés pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Assurez-vous que toutes les erreurs et avertissements du compilateur sont résolus. Les assemblys contenant les routines CLR peuvent ensuite être inscrits dans une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à l'aide de Visual Studio ou d'instructions [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
> [!NOTE]  
>  Le protocole réseau TCP/IP doit être activé sur l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] afin d'utiliser [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual Studio pour le développement et le débogage distants. Pour plus d’informations sur l’activation du protocole TCP/IP sur le serveur, consultez [configurer des protocoles clients](../../database-engine/configure-windows/configure-client-protocols.md).  
  
#### <a name="to-deploy-the-assembly-using-visual-studio"></a>Pour déployer l'assembly à l'aide de Visual Studio  
  
1.  Générez le projet en sélectionnant **générer** \<nom du projet > à partir de la **générer** menu.  
  
2.  Résolvez tous les avertissements et erreurs de build avant de déployer l'assembly sur le serveur de test.  
  
3.  Sélectionnez **déployer** à partir de la **générer** menu. L'assembly sera ensuite inscrit dans l'instance et la base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] spécifiées lors de la création initiale du projet [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans Visual Studio.  
  
#### <a name="to-deploy-the-assembly-using-transact-sql"></a>Pour déployer l'assembly à l'aide de Transact-SQL  
  
1.  Compilez l'assembly à partir du fichier source à l'aide des compilateurs de ligne de commande fournis avec le .NET Framework.  
  
2.  Pour les fichiers sources [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual C# :  
  
3.  `csc /target:library C:\helloworld.cs`  
  
4.  Pour les fichiers sources [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual Basic :  
  
 `vbc /target:library C:\helloworld.vb`  
  
 Ces commandes lancent le Visual c# ou à l’aide du compilateur Visual Basic le **/target** permettant de spécifier la génération d’une bibliothèque DLL.  
  
1.  Résolvez tous les avertissements et erreurs de build avant de déployer l'assembly sur le serveur de test.  
  
2.  Ouvrez [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] sur le serveur de test. Créez une requête, connectée à une base de données de test appropriée (tel qu'AdventureWorks).  
  
3.  Créez l'assembly dans le serveur en ajoutant le code [!INCLUDE[tsql](../../includes/tsql-md.md)] suivant à la requête.  
  
 `CREATE ASSEMBLY HelloWorld from 'c:\helloworld.dll' WITH PERMISSION_SET = SAFE;`  
  
1.  La procédure, fonction, agrégat, type défini par l'utilisateur ou déclencheur doit ensuite être créé(e) dans l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si le **HelloWorld** assembly contient une méthode nommée **HelloWorld** dans les **procédures** classe, ce qui suit [!INCLUDE[tsql](../../includes/tsql-md.md)] peuvent être ajoutés à la requête pour créer une procédure appelée **hello** dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 `CREATE PROCEDURE hello`  
  
 `AS`  
  
 `EXTERNAL NAME HelloWorld.Procedures.HelloWorld`  
  
 Pour plus d’informations sur la création des différents types d’objets de base de données managés dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [les fonctions](../../relational-databases/clr-integration-database-objects-user-defined-functions/clr-user-defined-functions.md), [les agrégats](../../relational-databases/clr-integration-database-objects-user-defined-functions/clr-user-defined-aggregates.md), [les Types](../../relational-databases/clr-integration-database-objects-user-defined-types/clr-user-defined-types.md), [procédures stockées CLR](http://msdn.microsoft.com/library/bbdd51b2-a9b4-4916-ba6f-7957ac6c3f33), et [déclencheurs CLR](http://msdn.microsoft.com/library/302a4e4a-3172-42b6-9cc0-4a971ab49c1c).  
  
## <a name="deploying-the-assembly-to-production-servers"></a>Déploiement de l'assembly sur des serveurs de production  
 Une fois que les objets de base de données CLR ont été testés et vérifiés sur le serveur de test, ils peuvent être distribués sur les serveurs de production. Pour plus d’informations sur le débogage des objets de base de données managés, consultez [débogage des objets de base de données CLR](../../relational-databases/clr-integration/debugging-clr-database-objects.md).  
  
 Le déploiement d'objets de base de données managés est semblable à celui des objets de base de données normaux (tables, routines [!INCLUDE[tsql](../../includes/tsql-md.md)], et ainsi de suite). Les assemblys contenant les objets de base de données CLR peuvent être déployés sur d'autres serveurs à l'aide d'un script de déploiement. Le script de déploiement peut être généré à l'aide de la fonctionnalité « Générer des scripts » de [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]. Le script de déploiement peut également être généré manuellement, ou généré à l'aide de « Générer des scripts » et modifié manuellement. Une fois le script de déploiement généré, il peut être exécuté sur d'autres instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour déployer les objets de base de données managés.  
  
#### <a name="to-generate-a-deployment-script-using-generate-scripts"></a>Pour générer un script de déploiement à l'aide de « Générer des scripts »  
  
1.  Ouvrez [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] et connectez-vous à l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] où l'assembly managé ou objet de base de données à déployer est inscrit.  
  
2.  Dans le **l’Explorateur d’objets**, développez le  **\<nom du serveur >** et **bases de données** arborescences. Avec le bouton droit de la base de données où l’objet de base de données managé est inscrit, sélectionnez **tâches**, puis sélectionnez **générer des Scripts**. L'Assistant Script s'ouvre.  
  
3.  Sélectionnez la base de données à partir de la zone de liste et cliquez sur **suivant**.  
  
4.  Dans le **choisir les Options de Script** volet, cliquez sur **suivant**, ou modifier les options, puis sur **suivant**.  
  
5.  Dans le **sélectionner les Types d’objet** volet, choisissez le type d’objet de base de données à déployer. Cliquez sur **Suivant**.  
  
6.  Pour chaque type d’objet sélectionné dans le **sélectionner les Types d’objet** volet, un **choisir \<type >** volet est présenté. Dans ce volet, vous pouvez choisir parmi toutes les instances de ce type d'objet de base de données inscrites dans la base de données spécifiée. Sélectionnez un ou plusieurs objets, cliquez sur **suivant**.  
  
7.  Le **Options de sortie** volet s’affiche lorsque toutes de la base de données souhaitée de l’objet type a été sélectionné. Sélectionnez **Script dans le fichier** et spécifiez un chemin d’accès de fichier pour le script. Sélectionnez **Suivant**. Passez en revue vos sélections et cliquez sur **Terminer**. Le script de déploiement est enregistré dans le chemin d'accès relatif spécifié.  
  
## <a name="post-deployment-scripts"></a>Scripts de post-déploiement  
 Vous pouvez exécuter un script de post-déploiement.  
  
 Pour ajouter un script de post-déploiement, ajoutez un fichier nommé postdeployscript.sql dans votre répertoire de projet Visual Studio. Par exemple, cliquez avec le bouton droit sur votre projet dans **l’Explorateur de solutions** et sélectionnez **ajouter un élément existant**. Ajoutez le fichier à la racine du projet, plutôt que dans le dossier Scripts de test.  
  
 Lorsque vous cliquez sur Déployer, Visual Studio exécute ce script après le déploiement de votre projet.  
  
## <a name="see-also"></a>Voir aussi  
 [Concepts de programmation pour l’intégration du CLR &#40;Common Language Runtime&#41;](../../relational-databases/clr-integration/common-language-runtime-clr-integration-programming-concepts.md)  
  
  
