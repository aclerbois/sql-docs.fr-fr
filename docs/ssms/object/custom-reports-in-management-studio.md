---
title: Rapports personnalisés dans Management Studio | Microsoft Docs
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
- sql13.swb.summary.new.custom.report.f1
helpviewer_keywords:
- SQL Server Management Studio [SQL Server], custom reports
ms.assetid: 1ba3f758-f39b-4f5f-91ca-516cedc78979
caps.latest.revision: 5
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: b9f7b9bd64829a337fc64639d0f65709df54336a
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="custom-reports-in-management-studio"></a>Rapports personnalisés dans Management Studio
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
Dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull_md.md)], un grand nombre de nœuds de l’Explorateur d’objets proposent un ensemble de rapports standard créés par [!INCLUDE[msCoName](../../includes/msconame_md.md)]. Ces rapports fournissent généralement les informations serveur demandées. À partir de [!INCLUDE[ssVersion2005](../../includes/ssversion2005_md.md)] Service Pack 2, les administrateurs peuvent exécuter des rapports personnalisés qui ont été créés dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull_md.md)] à partir de [!INCLUDE[ssManStudio](../../includes/ssmanstudio_md.md)].  
  
## <a name="implementation"></a>Implémentation  
Les rapports personnalisés sont stockés sous la forme de fichiers de définition de rapport (.rdl) et créés à l'aide du langage RDL (Report Definition Language). Le langage RDL dévoile les informations d'extraction de données et de disposition d'un rapport au format XML. Le langage RDL est un schéma ouvert Les développeurs peuvent l'étendre au moyen d'attributs et d'éléments supplémentaires. Les rapports peuvent exécuter toutes les instructions [!INCLUDE[tsql](../../includes/tsql_md.md)] valides au sein du rapport.  
  
Si l'Explorateur d'objets est connecté à un serveur, les rapports personnalisés ont la possibilité d'exécuter le contexte de la sélection en cours de l'Explorateur d'objets si les rapports font référence à des paramètres de rapport du nœud concerné. Un rapport peut ainsi exploiter le contexte actuel (par exemple, la base de données actuelle) ou bien un contexte cohérent, notamment en spécifiant la base de données désignée comme faisant partie de l'instruction [!INCLUDE[tsql](../../includes/tsql_md.md)] figurant dans le rapport personnalisé.  
  
## <a name="running-a-custom-report"></a>Exécution d'un rapport personnalisé  
Vous pouvez exécuter un rapport personnalisé [!INCLUDE[ssManStudio](../../includes/ssmanstudio_md.md)] des manières suivantes :  
  
-   Cliquez avec le bouton droit dans l’Explorateur d’objets, pointez sur **Rapports** et cliquez avec le bouton gauche sur **Rapports personnalisés**. Dans la boîte de dialogue **Ouvrir le fichier** , recherchez un dossier contenant des fichiers .rdl, puis ouvrez le fichier de rapport approprié.  
  
-   Cliquez avec le bouton droit sur un nœud dans l’Explorateur d’objets, pointez successivement sur **Rapports**et sur **Rapports personnalisés**, puis sélectionnez un rapport personnalisé dans la liste des fichiers récemment utilisés.  
  
## <a name="limitations"></a>Limitations  
Lorsque vous travaillez avec des rapports personnalisés, tenez compte des contraintes suivantes :  
  
-   Pour éviter toute exécution imprévisible d'un code malveillant, vous ne pouvez pas configurer [!INCLUDE[ssManStudio](../../includes/ssmanstudio_md.md)] en vue d'exécuter automatiquement un rapport, même si la configuration du système de fichiers autorise l'association de fichiers .rdl avec [!INCLUDE[ssManStudio](../../includes/ssmanstudio_md.md)]. Les rapports ne peuvent être ni exécutés par programme [!INCLUDE[ssManStudio](../../includes/ssmanstudio_md.md)] , ni exécutés à partir de la ligne de commande via [!INCLUDE[ssManStudio](../../includes/ssmanstudio_md.md)].  
  
-   Vous pouvez exécuter des rapports personnalisés dans un contexte qui ne produit pas les valeurs attendues. Par exemple, il vous est possible d'exécuter un rapport sur la réplication dans le contexte d'une base de données non impliquée dans la réplication ou bien d'exécuter un rapport en tant qu'utilisateur ne bénéficiant pas des autorisations d'accès aux informations requises pour la création d'un rapport exact. L'auteur du rapport personnalisé est responsable de la validité de la structure du rapport et de son contexte.  
  
-   Vous ne pouvez pas ajouter un rapport personnalisé à la liste des rapports standard.  
  
-   Le code traité par le rapport peut affecter les performances du serveur.  
  
-   Les rapports personnalisés ne prennent pas en charge de sous-rapports.  
  
-   Le texte de la commande de chaque requête au sein du rapport ne doit pas être défini par le biais d'une expression.  
  
-   Tous les paramètres de requête employés dans une commande (requête) peuvent uniquement faire référence à un paramètre de rapport unique et ne peuvent utiliser des opérateurs d'expression.  
  
-   Seuls les types de commande de texte et de procédure stockée sont pris en charge dans les commandes (requêtes) des rapports.  
  
-   La structure des rapports ne permet aucun échappement des paramètres des requêtes. Les auteurs des requêtes doivent s'assurer que leurs requêtes sont exemptes d'attaques par injection SQL.  
  
## <a name="managing-custom-reports"></a>Gestion des rapports personnalisés  
Nous recommandons aux utilisateurs qui possèdent un grand nombre de rapports personnalisés de les organiser à l'aide de dossiers de système de fichiers dotés des autorisations du système de fichiers NTFS appropriées.  
  
## <a name="permissions"></a>Autorisations  
Les rapports personnalisés sont exécutés avec les autorisations de l'utilisateur actuel. Pour empêcher tout utilisateur malveillant de modifier les requêtes exécutées par le rapport, les autorisations définies dans le dossier de système de fichiers contenant les fichiers de rapports doivent l'être avec un accès restreint.  
  
L'utilisateur tout comme le compte utilisé par le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] doivent bénéficier d'un accès en lecture au dossier de système de fichiers contenant les fichiers de rapports.  
  
Toutes les commandes [!INCLUDE[dnprdnshort](../../includes/dnprdnshort_md.md)] valides peuvent être incorporées dans un rapport mais ne seront pas exécutées.  
  
> [!CAUTION]  
> Toute instruction [!INCLUDE[tsql](../../includes/tsql_md.md)] valide peut être incorporée dans un rapport et exécutée depuis ce rapport. L'exécution d'un rapport avec un compte d'utilisateur doté de privilèges élevés permet d'exécuter sans aucune difficulté toutes ces instructions incorporées.  
  

  
## <a name="see-also"></a> Voir aussi  
[Ajouter un rapport personnalisé à Management Studio](../../ssms/object/add-a-custom-report-to-management-studio.md)  
[Annuler la suppression des avertissements d'exécution de rapports personnalisés](../../ssms/object/unsuppress-run-custom-report-warnings.md)  
[Utiliser des rapports personnalisés avec les propriétés de nœud de l’Explorateur d’objets](../../ssms/object/use-custom-reports-with-object-explorer-node-properties.md)  
  
