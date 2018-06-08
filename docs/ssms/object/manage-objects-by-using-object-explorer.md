---
title: Gérer les objets à l’aide de l’Explorateur d’objets | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssms-objects
ms.reviewer: ''
ms.suite: sql
ms.technology: ssms
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.SWB.SQLSERVEROBJECTEXPLORER.DHELP
helpviewer_keywords:
- Object Explorer F1 Help
- OE F1 Help
- OE Help
ms.assetid: e60367a7-3fdd-40b8-82bb-9e819d78de5a
caps.latest.revision: 5
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: 1ed3b055f684a0dfc7b3932c1a0d9aff318cf98e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="manage-objects-by-using-object-explorer"></a>Gérer les objets à l'aide de l'Explorateur d'objets
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
Vous pouvez utiliser l'Explorateur d'objets pour gérer des objets tels que les bases de données, les tables et les procédures stockées.  
  
## <a name="viewing-objects-in-object-explorer"></a>Affichage des objets dans l'Explorateur d'objets  
L'Explorateur utilise une arborescence pour regrouper les informations dans des dossiers. Pour développer un dossier, cliquez sur le signe plus (+) ou double-cliquez sur un dossier. Développez les dossiers pour afficher plus d'informations détaillées. Cliquez avec le bouton droit sur les dossiers ou les objets pour réaliser des tâches courantes. Double-cliquez sur les objets pour réaliser les tâches les plus courantes.  
  
La première fois que vous développez un dossier, l'Explorateur d'objets interroge le serveur pour obtenir des informations afin de remplir l'arbre. Vous pouvez effectuer d'autres opérations pendant le remplissage de l'arbre. Pendant le remplissage de l'arbre, vous pouvez cliquer sur **Arrêter** pour arrêter le processus. D'autres actions, telles que le filtrage de la liste, ne seront effectuées que sur la partie du dossier qui a été remplie, sauf si vous actualisez le dossier pour recommencer le remplissage.  
  
Pour conserver des ressources lorsqu'il existe beaucoup d'objets, les dossiers de l'Explorateur d'objets n'actualisent pas automatiquement leur liste de contenu. Pour actualiser la liste des objets dans un dossier, cliquez avec le bouton droit sur celui-ci, puis cliquez sur **Actualiser**.  
  
L'Explorateur d'objets peut afficher jusqu'à 65 536 objets. Lorsque ce nombre est dépassé, vous ne pouvez pas afficher d'objets supplémentaires dans l'arborescence. Pour afficher des objets supplémentaires dans l'Explorateur d'objets, fermez les nœuds inutilisés ou appliquez un filtre pour réduire le nombre d'objets.  
  
## <a name="filtering-the-list-of-objects-in-object-explorer"></a>Filtrage de la liste des objets dans l'Explorateur d'objets  
Lorsqu'un dossier contient un grand nombre d'objets, il peut être difficile de trouver l'objet que vous recherchez. Dans ce cas, utilisez la fonctionnalité de filtre de l'Explorateur d'objets pour réduire la taille de la liste. Par exemple, il est possible que vous souhaitiez trouver un utilisateur de base de données en particulier ou la table la plus récemment créée dans des listes qui contiennent des centaines d'objets. Dans ce cas, cliquez sur le dossier à filtrer, puis sur le bouton de filtre pour ouvrir la boîte de dialogue **Paramètres du filtre** . Vous pouvez filtrer la liste par nom, date de création, et parfois par schéma. Vous pouvez également fournir d'autres opérateurs comme **Commence par**, **Contient**et **Entre**.  
  
## <a name="multi-select"></a>Multisélection  
Dans l'Explorateur d'objets, il n'est possible de sélectionner qu'un seul objet à la fois. Pour sélectionner plusieurs éléments, appuyez sur **F7** pour ouvrir la page **Détails de l'Explorateur d'objets**. La page **Détails de l’Explorateur d’objets** prend en charge la multisélection.  
  
## <a name="register-a-server-from-object-explorer"></a>Inscrire un serveur à partir de l'Explorateur d'objets  
Lorsque vous êtes connecté à un serveur, vous pouvez facilement inscrire le serveur pour une future utilisation. Dans l’Explorateur d’objets, cliquez avec le bouton droit sur le nom du serveur, puis cliquez sur **Inscrire**. Dans la boîte de dialogue **Inscrire un serveur** , indiquez où vous souhaitez placer le serveur dans l'arborescence des serveurs. Dans la zone **Nom du serveur** , vous pouvez remplacer le nom du serveur par un nom plus explicite. Par exemple, vous pourriez inscrire le serveur **CFSQL02** sous le nom plus explicite de**Comptes fournisseurs**.  
  
## <a name="performing-actions-on-object-explorer-nodes"></a>Exécution d'actions sur des nœuds de l'Explorateur d'objets  
Vous exécutez des actions sur les objets en cliquant avec le bouton droit sur le nœud de l'Explorateur d'objets qui représente l'objet. Chaque type d'objet prend en charge un ensemble unique d'actions associées au menu contextuel. Voici certains types d'actions que vous pouvez effectuer à l'aide des menus contextuels :  
  
### <a name="open-a-connected-query-editor"></a>Ouvrir un éditeur de requête connecté  
Lorsque l'Explorateur d'objets est connecté à un serveur, vous pouvez ouvrir une nouvelle fenêtre d'éditeur de code à l'aide des paramètres de connexion de l'Explorateur d'objets. Pour ouvrir une nouvelle fenêtre d’éditeur de code, cliquez avec le bouton droit sur le nom du serveur dans l’Explorateur d’objets, puis cliquez sur **Nouvelle requête**. Pour ouvrir une fenêtre d’éditeur à l’aide d’une base de données, cliquez avec le bouton droit sur le nom de la base de données, puis cliquez sur **Nouvelle requête**. Lorsque vous ouvrez une nouvelle requête pour un serveur Analysis Services, vous pouvez sélectionner des requêtes DMX, MDX ou XMLA.  
  
### <a name="start-powershell"></a>Démarrer PowerShell  
Vous pouvez démarrer une session PowerShell en cliquant avec le bouton droit sur la plupart des dossiers et objets dans l’arborescence de l’Explorateur d’objets et en sélectionnant **Démarrer PowerShell**. Cette opération démarre une session PowerShell pour laquelle la prise en charge [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] PowerShell est activée, et dont le chemin est défini sur l’objet sur lequel vous avez cliqué avec le bouton droit dans l’Explorateur d’objets. Vous pouvez entrer ensuite des commandes PowerShell dans un environnement PowerShell interactif. Pour en savoir plus, consultez [SQL Server PowerShell](http://msdn.microsoft.com/en-us/89b70725-bbe7-4ffe-a27d-2a40005a97e7).  
  
## <a name="see-also"></a> Voir aussi  
[l’Explorateur d’objets](../../ssms/object/object-explorer.md)  
[Ouvrir et configurer l’Explorateur d’objets](../../ssms/object/open-and-configure-object-explorer.md)  
[Se connecter à une instance à partir de l’Explorateur d’objets](../../ssms/object/connect-to-an-instance-from-object-explorer.md)  
[Volet Détails de l'Explorateur d'objets](../../ssms/object/object-explorer-details-pane.md)  
[Rapports personnalisés dans Management Studio](../../ssms/object/custom-reports-in-management-studio.md)  
  
