---
title: Plan de maintenance (onglet Conception) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: maintenance-plans
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.swb.maint.maintplanproperties.optimizations.f1
- sql13.swb.maint.planeditor.f1
- sql13.swb.maint.subplaneditor.f1
ms.assetid: 6d20d4d4-5b3f-454a-8a05-f0aac803c5ad
caps.latest.revision: 27
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 2459b8508cc7b69225b21aef96148978897d3ff7
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="maintenance-plan-design-tab"></a>Plan de maintenance, onglet Conception
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Utilisez **Plan de maintenance (onglet Conception)** pour spécifier les propriétés d’un plan de maintenance et de ses sous-plans. Faites glisser des tâches de la barre d'outils jusqu'au concepteur de plan. Cliquez avec le bouton droit sur des groupes de tâches pour créer des branchements de chemins d'exécution. Les plans de maintenance sont enregistrés en tant que packages [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] qui sont exécutés par les travaux de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="options"></a>Options  
 **Ajouter le sous-plan**  
 Ajoutez un sous-plan que vous pouvez configurer.  
  
 **Propriétés du sous-plan**  
 Affichez la boîte de dialogue **Propriétés du sous-plan** . Sélectionnez un sous-plan dans la grille et cliquez sur cette icône pour entrer un nom, une description et planifier le sous-plan. Vous pouvez aussi double-cliquer sur le sous-plan dans la grille pour afficher la boîte de dialogue **Propriétés du sous-plan** . Les noms du sous-plan sont limités à 128 caractères et leurs descriptions sont limitées à 512 caractères.  
  
 **Supprimer le sous-plan sélectionné**  
 Supprimez le sous-plan sélectionné.  
  
 **Planification du sous-plan**  
 Affichez la boîte de dialogue **Propriétés de la planification du travail** . Sélectionnez un sous-plan dans la grille et cliquez sur cette icône pour configurer une planification pour le sous-plan.  
  
 **Supprimer la planification**  
 Supprimez une planification dans les planifications sélectionnées.  
  
 **Gérer les connexion**  
 Affichez la boîte de dialogue **Gérer les connexions** . Permet d'ajouter des connexions d'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] supplémentaires au plan de maintenance. Chaque tâche de maintenance dans l'éditeur de sous-plan peut utiliser ces connexions. Lors de son exécution, le plan de maintenance établit une connexion entre le serveur du plan de maintenance et les serveurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] spécifiés, à l'aide des informations d'identification de connexion.  
  
 **Création de rapport et enregistrement**  
 Affichez la boîte de dialogue **Création de rapport et enregistrement** qui permet de gérer les rapports relatifs à l’activité du plan de maintenance et de configurer l’enregistrement sur le serveur local ou distant.  
  
 **Serveurs**  
 Affichez la boîte de dialogue **Serveurs** qui permet de sélectionner les serveurs où sont exécutées les tâches du sous-plan. Cette option est activée uniquement sur des serveurs maîtres dans des environnements multiserveurs. Pour plus d’informations, consultez [Créer un environnement multiserveur](http://msdn.microsoft.com/library/edc2b60d-15da-40a1-8ba3-f1d473366ee6).  
  
 **Nom**  
 Affichez le nom du plan de maintenance. Pour les nouveaux plans de maintenance, le nom est spécifié dans une boîte de dialogue avant l'ouverture du concepteur de plan de maintenance. Pour renommer un plan de maintenance, cliquez dessus avec le bouton droit dans l’Explorateur d’objets, puis cliquez sur **Renommer**.  
  
 **Description**  
 Permet d'afficher ou de spécifier une description du plan de maintenance. La longueur maximale de la description est 512 caractères.  
  
 **Aire du concepteur**  
 Conception et entretien des plans de maintenance. Utilisez l'aire du concepteur pour ajouter des tâches de maintenance à un plan, supprimer des tâches d'un plan, spécifier des liens de précédence entre des tâches et indiquer les branchements et le parallélisme des tâches.  
  
 Un lien de précédence entre deux tâches établit une relation entre ces tâches. La seconde tâche (la *tâche dépendante*) s’exécute seulement si le résultat d’exécution de la première tâche (la *tâche prioritaire*) correspond aux critères spécifiés. En général, le résultat d'exécution spécifié est **Succès**, **Échec**ou **À l'achèvement**. L'aire du concepteur de plan de maintenance est basée sur l'aire du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] . Pour plus d’informations, consultez [Contraintes de précédence](../../integration-services/control-flow/precedence-constraints.md).  
  
 Comme exemple, il est possible de spécifier une tâche Défragmenter l'index qui doit s'exécuter seulement si une tâche Vérifier l'intégrité de la base de données antérieure s'est terminée correctement. La fonctionnalité de liaison des tâches par priorités permet également de traiter les conditions d'erreur ou d'échec dans un plan. Par exemple, si la tâche Vérifier l'intégrité de la base de données a échoué, une tâche Notifier l'opérateur peut informer un utilisateur ou un opérateur de l'échec.  
  
 La spécification de tâches à exécuter suite à l’échec d’une tâche précédente est un exemple de *branchement de tâche*.  
  
 Indiquer que deux tâches ou plus doivent commencer à s’exécuter simultanément, par exemple après la réussite d’une tâche précédente, est un exemple de définition du *parallélisme des tâches*. Tous les tâches sans contrainte démarrent et s'exécutent en parallèle. Les contraintes vous permettent de retarder certaines tâches pour laisser s'achever d'autres tâches en premier.  
  
 Une fois qu'une tâche de maintenance est placée sur la surface de dessin, ses propriétés peuvent être modifiées de manière appropriée. Par exemple, la base de données spécifique à sauvegarder dans une Tâche Sauvegarder la base de données est spécifiée après que la tâche a été ajoutée au plan. Les tâches sur la surface de dessin qui ne sont pas configurées correctement contiennent une icône rouge avec un x blanc.  
  
 Pour ajouter une tâche de maintenance dans un plan, faites glisser l’icône de la tâche à partir de la boîte à outils **Tâches du plan de maintenance** vers la surface de dessin, ou double-cliquez sur la tâche dans la boîte à outils qui ajoute cette tâche à l’aire du concepteur actif. Si la boîte à outils **Tâches du plan de maintenance** n’est pas affichée, cliquez sur **Boîte à outils** dans le menu [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] **View** menu. Développez le nœud **Tâches du plan de maintenance** dans le volet **Boîte à outils** .  
  
 Pour supprimer une tâche d’un plan, sélectionnez la tâche dans l’aire du concepteur et appuyez sur la touche **Suppr** ou cliquez avec le bouton droit sur la tâche et cliquez sur **Supprimer**.  
  
 Pour spécifier des liens de précédence entre deux tâches, commencez par faire glisser les tâches sur la surface de dessin, puis cliquez sur la tâche qui apparaît en premier (tâche précédente) et faites glisser la flèche jusqu'à la tâche dépendante. Une fois qu'un lien de précédence a été établi, le concepteur affiche une flèche reliant les deux tâches, de la tâche prioritaire vers la tâche dépendante. Par défaut, quand un lien est établi au préalable, la contrainte du lien est définie de telle sorte que la tâche dépendante s’exécute uniquement si le résultat de l’exécution de la tâche prioritaire est **Succès**.  
  
 Pour modifier les propriétés d’un lien de précédence, double-cliquez sur le lien pour démarrer l’ **Éditeur de contrainte de précédence**. Cela fournit de nombreuses options pour spécifier les conditions logiques qui déterminent si la tâche dépendante s'exécute. Par exemple, le **Résultat d’exécution** peut prendre la valeur **Échec**, auquel cas la tâche dépendante s’exécute uniquement si la tâche prioritaire échoue. Il est également possible de modifier la propriété du résultat d’exécution d’un lien en spécifiant **Succès**, **Échec**ou **À l’achèvement**en cliquant avec le bouton droit sur le lien, puis en effectuant une sélection dans le menu contextuel.  
  
 Pour spécifier un branchement de tâche, commencez par créer des liens de précédence entre deux tâches. Ensuite, placez une autre tâche dépendante sur la surface de dessin, qui s'exécute sur un autre résultat que la première tâche dépendante. Cliquez sur la tâche précédente et faites glisser la seconde flèche à partir de la tâche prioritaire vers la tâche dépendante. Pour modifier le résultat d’exécution (**Succès**, **Échec**, **À l’achèvement**) qui entraîne l’exécution d’une tâche dépendante, double-cliquez sur la flèche du lien et modifiez le champ **Résultat d’exécution** . Vous pouvez également cliquer avec le bouton droit sur le lien et sélectionner la valeur du résultat d'exécution souhaitée dans le menu contextuel.  
  
 Pour spécifier le parallélisme des tâches, liez deux tâches dépendantes ou plus à une tâche prioritaire unique. Modifiez les propriétés des liens de précédence de sorte que les champs Résultat d'exécution des liens qui pointent sur les tâches dépendantes qui s'exécutent en parallèle ont la même valeur.  
  
## <a name="additional-features-available-from-the-shortcut-menu"></a>Fonctionnalités supplémentaires disponibles à partir du menu contextuel  
 Pour afficher des options supplémentaires, sélectionnez une ou plusieurs tâches sur la surface de dessin, puis cliquez avec le bouton droit pour afficher le menu contextuel. Outre les options classiques **Couper**, **Copier**, **Coller**, **Supprimer**et **Tout sélectionner**, les options spéciales ci-après sont disponibles pour certaines tâches.  
  
 **Ajouter une annotation**  
 Ajoute une note descriptive sur l’aire de conception.  
  
 **Modifier**  
 Ouvre la boîte de dialogue de propriétés de la tâche.  
  
 **Désactiver**  
 Rend la tâche provisoirement indisponible.  
  
 **Activer**  
 Restaure une tâche désactivée.  
  
 **Grouper**  
 Crée un groupe contenant une ou plusieurs tâches.  
  
 **Dissocier**  
 Supprime les tâches d'un groupe.  
  
 **Redimensionnement automatique**  
 Dimensionne chaque tâche en utilisant la taille optimale pour cette tâche.  
  
 **Réduire**  
 Masque les tâches au sein d’un groupe.  
  
 **Développer**  
 Montre les tâches d’un groupe auparavant masquées à l’aide de l’option **Réduire**.  
  
 **Zoom**  
 Modifie la taille des tâches sur l’aire de conception  
  
## <a name="see-also"></a> Voir aussi  
 [Plans de maintenance](../../relational-databases/maintenance-plans/maintenance-plans.md)   
 [Créer un plan de maintenance](../../relational-databases/maintenance-plans/create-a-maintenance-plan.md)  
  
  
