---
title: Fonction classifieur de Resource Governor | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: performance
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- Resource Governor, classifier function
- user-defined functions [SQL Server], classifier function
- classifier function [SQL Server]
- classifier function [SQL Server], overview
ms.assetid: 64c25012-7068-476f-afa2-0b4f3adde9a4
caps.latest.revision: 7
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 0b74c5eb74c2171c7a3c036e085140d0738b9a43
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="resource-governor-classifier-function"></a>Fonction classifieur du gouverneur de ressources
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Le processus de classification de Resource Governor [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] affecte les sessions entrantes à un groupe de charge de travail en fonction des caractéristiques de la session. Vous pouvez adapter la logique de classification en entrant une fonction définie par l'utilisateur, appelée fonction classifieur.  
  
## <a name="classification"></a>classification.  
 Resource Governor prend en charge la classification des sessions entrantes. La classification est basée sur un jeu de critères écrits par l'utilisateur et contenus dans une fonction. Les résultats de la logique de la fonction permettent à Resource Governor de classer des sessions en groupes de charges de travail existants.  
  
> [!NOTE]  
>  Le groupe de charges de travail interne est rempli avec les demandes destinées à un usage interne uniquement. Vous ne pouvez pas modifier les critères utilisés pour acheminer ces demandes et vous ne pouvez pas classer de demandes dans le groupe de charges de travail interne.  
  
 Vous pouvez écrire une fonction scalaire qui contient la logique utilisée pour assigner des sessions entrantes à un groupe de charges de travail. Avant de pouvoir utiliser cette fonction, vous devez effectuer les opérations suivantes :  
  
-   Créer et enregistrer la fonction à l'aide de l'instruction ALTER RESOURCE GOVERNOR. Pour plus d’informations, consultez [ALTER RESOURCE GOVERNOR &#40;Transact-SQL&#41;](../../t-sql/statements/alter-resource-governor-transact-sql.md).  
  
-   Mettre à jour la configuration de Resource Governor à l'aide de l'instruction ALTER RESOURCE GOVERNOR avec le paramètre RECONFIGURE.  
  
 Une fois la fonction créé et les modifications de configuration appliquées, le classifieur de Resource Governor utilise le nom du groupe de charges de travail retourné par la fonction pour envoyer une nouvelle demande au groupe de charge de travail approprié.  
  
> [!IMPORTANT]  
>  La session cliente peut expirer si la fonction de classification ne se termine pas dans le délai d'attente spécifié pour la connexion. Le délai d'attente de connexion est une propriété cliente et à ce titre, le serveur n'en a pas connaissance. Une fonction classifieur de longue durée peut entraîner des connexions orphelines au niveau du serveur pendant de longues périodes. Il est important de créer des fonctions classifieur dont l'exécution se termine avant l'expiration d'un délai de connexion.  
  
 La fonction définie par l'utilisateur a les caractéristiques et les comportements suivants :  
  
-   La fonction définie par l'utilisateur est évaluée pour chaque nouvelle session, même lorsque le regroupement de connexions est activé.  
  
-   La fonction définie par l'utilisateur donne le contexte de groupe de charges de travail pour la session. Une fois l'appartenance aux groupes déterminée, la session est liée au groupe de charges de travail pour la durée de la session.  
  
-   Si la fonction définie par l'utilisateur retourne NULL, la valeur par défaut ou le nom de groupe inexistant, elle se voit attribuer le contexte de groupe de charges de travail par défaut. La session se voit attribuer aussi le contexte par défaut si la fonction échoue pour une raison donnée.  
  
-   La fonction doit être définie avec une étendue de serveur (base de données master).  
  
-   La désignation de la fonction classifieur définie par l'utilisateur entre seulement en vigueur après l'exécution de l'instruction ALTER RESOURCE GOVERNOR RECONFIGURE.  
  
-   Une seule fonction définie par l'utilisateur peut être désignée à la fois comme classifieur.  
  
-   La fonction classifieur définie par l'utilisateur ne peut pas être supprimée ou modifiée sauf si son état classifieur est supprimé.  
  
-   En l'absence d'une fonction classifieur définie par l'utilisateur, toutes les sessions sont classifiées dans le groupe par défaut.  
  
-   Le groupe de charges de travail retourné par la fonction classifieur est hors de la portée de la restriction de liaison du schéma. Par exemple, vous ne pouvez pas supprimer une table mais vous pouvez supprimer un groupe de charges de travail.  
  
> [!IMPORTANT]  
>  L'activation de la connexion administrateur dédiée (DAC) sur le serveur est conseillée. La connexion administrateur dédiée n'est pas soumise à la classification de Resource Governor et peut être utilisée pour surveiller et dépanner une fonction classifieur. Pour plus d’informations, consultez [Connexion de diagnostic pour les administrateurs de base de données](../../database-engine/configure-windows/diagnostic-connection-for-database-administrators.md). L'autre solution, lorsqu'une connexion administrateur dédiée n'est pas disponible pour la résolution des problèmes, consiste à redémarrer le système en mode mono-utilisateur. Le mode mono-utilisateur n'est pas sujet à la classification ; toutefois, il ne vous permet pas d'effectuer un diagnostic de la classification de Resource Governor s'il est en cours d'exécution.  
  
### <a name="classification-process"></a>Processus de classification  
 Dans le contexte de Resource Governor, le processus de connexion pour une session comprend les étapes suivantes :  
  
1.  authentification des connexions ;  
  
2.  exécution des déclencheurs LOGON ;  
  
3.  classification.  
  
 Lorsque la classification commence, Resource Governor exécute la fonction classifieur et utilise la valeur retournée par la fonction pour envoyer des demandes au groupe de charges de travail approprié.  
  
> [!NOTE]  
>  Les informations relatives à l’exécution de la fonction classifieur et des déclencheurs LOGON sont exposées dans [sys.dm_exec_sessions](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sessions-transact-sql.md) et [sys.dm_exec_requests](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md).  
  
## <a name="classification-function-tasks"></a>Tâches de fonction de classification  
  
|Description de la tâche|Rubrique|  
|----------------------|-----------|  
|Décrit comment créer et tester une fonction définie par l'utilisateur classifieur.|[Créer et tester une fonction classifieur définie par l'utilisateur](../../relational-databases/resource-governor/create-and-test-a-classifier-user-defined-function.md)|  
  
## <a name="see-also"></a> Voir aussi  
 [Resource Governor](../../relational-databases/resource-governor/resource-governor.md)   
 [Activer Resource Governor](../../relational-databases/resource-governor/enable-resource-governor.md)   
 [Pool de ressources de Resource Governor](../../relational-databases/resource-governor/resource-governor-resource-pool.md)   
 [Groupe de charge de travail de Resource Governor](../../relational-databases/resource-governor/resource-governor-workload-group.md)   
 [Configurer le gouverneur de ressources à l'aide d'un modèle](../../relational-databases/resource-governor/configure-resource-governor-using-a-template.md)   
 [Afficher les propriétés du gouverneur de ressources](../../relational-databases/resource-governor/view-resource-governor-properties.md)  
  
  
