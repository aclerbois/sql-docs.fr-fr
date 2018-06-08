---
title: Créer des Scripts Analysis Services dans Management Studio | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: ''
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 28b1b0068f9ddd9bf47bc2fe93177db469c8b4f1
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="create-analysis-services-scripts-in-management-studio"></a>Créer des scripts Analysis Services dans Management Studio
[!INCLUDE[ssas-appliesto-sqlas-all-aas](../../includes/ssas-appliesto-sqlas-all-aas.md)]

  [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] inclut des fonctionnalités de génération de script, des modèles et des éditeurs dont vous pouvez vous servir pour créer des scripts d'objets et de tâches Analysis Services.  
  
## <a name="script-analysis-services-tasks-in-management-studio"></a>Créer des scripts de tâches Analysis Services dans Management Studio  
 Les tâches de script dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] sont accomplies en cliquant sur l’une des options de script dans une boîte de dialogue orientée tâche. Toutes les boîtes de dialogue que vous utilisez pour effectuer des tâches telles que la sauvegarde ou la restauration de la base de données, pour traiter un objet, ou pour concevoir un regroupement, incluent une option de script en haut de la boîte de dialogue. La sélection de l'une de ces options génère un script XMLA en fonction des informations et des paramètres de la boîte de dialogue.  
  
 Par défaut, le script est généré et placé dans un éditeur de requête XMLA, mais vous pouvez également développer la liste des options de script pour exécuter le script dans le Presse-papiers Windows ou dans un fichier.  
  
#### <a name="to-script-an-analysis-services-task"></a>Pour créer un script de tâche Analysis Services  
  
1.  Dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], connectez-vous à l'instance [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].  
  
2.  Cliquez avec le bouton droit sur une base de données, puis cliquez sur **Sauvegarder**. La boîte de dialogue Sauvegarder la base de données s’ouvre. Spécifiez un nom de fichier de sauvegarde et sélectionnez les options souhaitées pour cette sauvegarde.  
  
3.  Cliquez sur **Script** en haut de la boîte de dialogue. La fonctionnalité Script fait partie de toutes les boîtes de dialogue basées sur les tâches dans Management Studio. Elle contient les options suivantes : **Action de script dans une nouvelle fenêtre de requête** pour ouvrir la fenêtre d’éditeur de requête, **Action de script vers le fichier** pour enregistrer le script XMLA dans un fichier, ou **Action de script vers le Presse-papiers** pour enregistrer le script XMLA dans le Presse-papiers.  
  
     Notez que l’option **Action de script vers le travail** , qui est répertoriée comme une option de script dans Management Studio, n’est pas prise en charge pour les scripts Analysis Services.  
  
4.  Si vous sélectionnez l’option par défaut, **Action de script dans une nouvelle fenêtre de requête**, un script généré est placé dans une fenêtre de requête XMLA.  
  
     Vous pouvez maintenant fermer la boîte de dialogue Sauvegarder la base de données et modifier ou exécuter le script XMLA directement.  
  
## <a name="script-analysis-services-objects-in-management-studio"></a>Créer un script pour des objets Analysis Services dans Management Studio  
 Dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] , la création de scripts pour des objets s’effectue en cliquant avec le bouton droit sur un objet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] et en sélectionnant **Create to**, **Alter to**ou **Delete to**. Chacune de ces options peut être dirigée vers une fenêtre ou un fichier, mais quel que soit l'endroit vers lequel il est dirigé, le script se présentera sous la forme d'un script DDL dans un wrapper XMLA. L'un des grands avantages de tels scripts est qu'ils vous permettent de les exécuter sur le serveur de votre choix. En outre, il est possible de modifier les noms figurant dans les scripts de façon itérative, ce qui permet la construction, la modification ou la suppression d'objets en masse.  
  
 Les objets pour lesquels vous pouvez créer un script incluent les éléments d’une base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] , notamment des sources de données, des vues de sources de données, des cubes, des dimensions, des structures d’exploration de données et des rôles.  
  
 Parmi les conditions préalables, vous devez savoir comment utiliser XML for Analysis (XMLA). Heureusement, [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] dispose d’une fonction qui crée automatiquement le script XMLA nécessaire pour créer des objets tels que des cubes. Cette fonction d’automatisation permet de réduire la durée d’apprentissage de XMLA. Pour plus d’informations sur l’utilisation de XMLA, consultez [Développement avec XMLA dans Analysis Services](../../analysis-services/multidimensional-models-scripting-language-assl-xmla/developing-with-xmla-in-analysis-services.md). Pour plus d’informations sur l’utilisation de XMLA, consultez [Développement avec XMLA dans Analysis Services](../../analysis-services/multidimensional-models-scripting-language-assl-xmla/developing-with-xmla-in-analysis-services.md).  
  
> [!IMPORTANT]  
>  Lorsque vous générez un script pour l'objet Rôle, sachez que les autorisations de sécurité sont contenues dans les objets qu'elles sécurisent, et non dans le rôle de sécurité auquel elles sont associées.  
  
#### <a name="to-script-analysis-services-objects"></a>Pour créer un script pour des objets Analysis Services  
  
1.  Dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], connectez-vous à l'instance [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].  
  
2.  Recherchez l'objet pour lequel vous voulez créer un script qui crée, modifie ou supprime des objets.  
  
3.  Cliquez avec le bouton droit sur l’objet et pointez sur **Générer un script du cube en tant que**, pointez sur **Create to**, **Alter to**ou **Delete to**, puis cliquez sur l’une des options suivantes : **Nouvelle fenêtre d’éditeur de requête** pour ouvrir la fenêtre d’éditeur de requête, **Fichier** pour enregistrer le script XMLA dans un fichier ou **Presse-papiers** pour enregistrer le script XMLA dans le Presse-papiers.  
  
    > [!NOTE]  
    >  Généralement, vous sélectionnez **Fichier** pour créer plusieurs versions différentes du fichier.  
  
## <a name="see-also"></a>Voir aussi  
 [Éditeur de requête XMLA & #40 ; Analysis Services - données multidimensionnelles & #41 ;](http://msdn.microsoft.com/library/14623019-7839-4038-9d12-2f8953d2ec04)  
  
  
