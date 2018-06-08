---
title: Aide sur l’Assistant Gestion de partition via la touche F1 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: partitions
ms.reviewer: ''
ms.suite: sql
ms.technology:
- dbe-partition
ms.tgt_pltfrm: ''
ms.topic: reference
f1_keywords:
- sql13.swb.managepartition.createjob.f1
- sql13.swb.managepartition.progress.f1
- sql13.swb.managepartition.getstart.f1
- sql13.swb.managepartition.selectswitchtables.f1
- sql13.swb.managepartition.stagingtable.f1
- sql13.swb.managepartition.switchin.f1
- sql13.swb.managepartition.switchout.f1
- sql13.swb.managepartition.partitionaction.f1
- sql13.swb.managepartition.summary.f1
- sql13.swb.managepartition.selectoutput.f1
helpviewer_keywords:
- wizards [SQL Server Management Studio] See Manage Partition Wizard
ms.assetid: e2478d26-dea4-428d-98c5-aad2d2a30da8
caps.latest.revision: 12
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 686001afb4411971d6bb278e5ca7db7e5b64598d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="manage-partition-wizard-f1-help"></a>Aide sur l'Assistant Gestion de partition via la touche F1
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Utilisez l’ **Assistant Gestion de partition** pour gérer et modifier des tables partitionnées existantes via le basculement de partition ou l’implémentation d’un scénario de fenêtre glissante. Cet Assistant peut faciliter la gestion de vos partitions et simplifier la migration régulière de données vers et à partir de vos tables.  
  
### <a name="to-start-the-manage-partition-wizard"></a>Pour démarrer l'Assistant Gestion de partition  
  
-   Dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], sélectionnez la base de données, cliquez avec le bouton droit sur la table sur laquelle vous souhaitez créer des partitions, pointez sur **Stockage**, puis cliquez sur **Gérer la partition**.  
  
     **Remarque** Si **Gérer la partition** n’est pas disponible, vous avez peut-être sélectionné une table qui ne contient pas de partitions. Cliquez sur **Créer la partition** dans le sous-menu **Stockage** et utilisez l’ **Assistant Création de partition** pour créer des partitions dans votre table.  
  
 Pour obtenir des informations générales sur les partitions et les index, consultez [Tables et index partitionnés](../../relational-databases/partitions/partitioned-tables-and-indexes.md).  
  
 Cette section fournit les informations requises pour gérer, modifier et implémenter des partitions à l’aide de l’ **Assistant Gestion de partition**.  
  
##  <a name="Top"></a> Dans cette section  
 Les sections ci-dessous fournissent de l’aide sur les pages de l’ **Assistant Gestion de partition**.  
  
 [Assistant Gestion de partition (page Sélectionnez une action de partition)](#SelectPartitionAction)  
  
 [Assistant Gestion de partition (page Insérer)](#SwitchIn)  
  
 [Assistant Gestion de partition (page Extraire)](#SwitchOut)  
  
 [Assistant Gestion de partition (page Sélectionner les options de table intermédiaire)](#StagingTableOptions)  
  
 [Assistant Gestion de partition (page Sélectionner une option de sortie)](#OutputOption)  
  
 [Assistant Gestion de partition (page Nouvelle planification du travail)](#NewJob)  
  
 [Assistant Gestion de partition (page Résumé)](#Summary)  
  
 [Assistant Gestion de partition (page Progression)](#Progress)  
  
##  <a name="SelectPartitionAction"></a> Page Sélectionnez une action de partition  
 Utilisez la page **Sélectionnez une action de partition** pour choisir l’action à effectuer sur votre partition.  
  
### <a name="create-a-staging-table"></a>Créer une table intermédiaire  
 Le basculement de partition est une tâche de partitionnement courante si vous disposez d'une table partitionnée vers ou à partir de laquelle vous faites régulièrement migrer des données ; il peut par exemple s'agir d'une table partitionnée qui stocke des données du trimestre actuel, que vous devez alimenter en nouvelles données et dont vous devez archiver les anciennes données à la fin de chaque trimestre.  
  
 L'Assistant conçoit la table intermédiaire avec les mêmes colonne de partitionnement, structure de tables et de colonnes, et index, et stocke la nouvelle table dans le groupe de fichiers où se trouve votre partition source.  
  
 Pour créer une table intermédiaire pour insérer ou extraire des données de partition, sélectionnez **Créer une table intermédiaire pour le basculement de partition**.  
  
### <a name="sliding-window-scenario"></a>Scénario de fenêtre glissante  
 Pour gérer vos partitions dans un scénario de fenêtre défilante, sélectionnez **Gérer les données partitionnées dans un scénario de fenêtre glissante**.  
  
## <a name="uielement-list"></a>Liste des éléments de l'interface utilisateur  
 **Créer une table intermédiaire pour le basculement de partition**  
 Crée une table intermédiaire pour les données que vous insérez ou extrayez dans la table partitionnée existante.  
  
 **Extraire une partition**  
 Fournit des options lors de la suppression d'une partition de la table.  
  
 **Insérer une partition**  
 Fournit des options lors de l'ajout d'une partition à la table.  
  
 **Gérer les données partitionnées dans un scénario de fenêtre glissante**  
 Ajoute à la table existante une partition vide qui peut être utilisée pour l'insertion de données. L'Assistant prend actuellement en charge l'insertion dans la dernière partition et l'extraction de la première partition.  
  
 ![Icône de flèche utilisée avec le lien Retour au début](../../analysis-services/instances/media/uparrow16x16.gif "Icône de flèche utilisée avec le lien Retour au début")[Dans cette section](#Top)  
  
##  <a name="SwitchIn"></a> Page Sélectionner les options d'insertion de partition  
 Utilisez la page **Sélectionner les options d’insertion de partition** pour sélectionner la table de mise en lots que vous insérez dans la table partitionnée.  
  
## <a name="uielement-list"></a>Liste des éléments de l'interface utilisateur  
 **Afficher toutes les partitions**  
 Sélectionnez cette option pour afficher toutes les partitions, notamment les partitions figurant actuellement dans la table partitionnée.  
  
 **Grille de partition**  
 Affiche le nom des partitions, ainsi que les informations **Limite gauche**, **Limite droite**, **Groupe de fichiers**et **Nombre de lignes** des partitions que vous avez sélectionnées.  
  
 **Table d'insertion**  
 Sélectionnez la table intermédiaire qui contient la partition que vous souhaitez ajouter à votre table partitionnée. Vous devez créer cette table de mise en lots avec l’ **Assistant Gestion de partition**avant d’insérer des partitions.  
  
 ![Icône de flèche utilisée avec le lien Retour au début](../../analysis-services/instances/media/uparrow16x16.gif "Icône de flèche utilisée avec le lien Retour au début")[Dans cette section](#Top)  
  
##  <a name="SwitchOut"></a> Page Sélectionner les options d'extraction de partition  
 Utilisez la page **Sélectionner les options d’extraction de partition** pour sélectionner la partition et la table de mise en lots destinées à maintenir les données partitionnées que vous extrayez de la table partitionnée.  
  
## <a name="uielement-list"></a>Liste des éléments de l'interface utilisateur  
 **Grille de partition**  
 Affiche le nom des partitions, ainsi que les informations **Limite gauche**, **Limite droite**, **Groupe de fichiers**et **Nombre de lignes** des partitions que vous avez sélectionnées.  
  
 **Table d'extraction**  
 Choisissez une nouvelle table ou une table existante vers laquelle extraire vos données.  
  
 **Nouveau**  
 Entrez un nouveau nom pour la table intermédiaire à utiliser pour la partition à extraire de la table source actuelle.  
  
 **Existant**  
 Sélectionnez la table intermédiaire à utiliser pour la partition à extraire de la table source actuelle. Si la table existante contient des données, ces données seront remplacées par celles que vous extrayez.  
  
 ![Icône de flèche utilisée avec le lien Retour au début](../../analysis-services/instances/media/uparrow16x16.gif "Icône de flèche utilisée avec le lien Retour au début")[Dans cette section](#Top)  
  
##  <a name="StagingTableOptions"></a> Page Sélectionner les options de table de mise en lots  
 Utilisez la page **Sélectionner les options de table de mise en lots** pour créer la table de mise en lots à utiliser pour basculer vos données partitionnées.  
  
 Les tables intermédiaires doivent résider dans le même groupe de fichiers que la partition sélectionnée où se trouve la table source. La table intermédiaire doit refléter la conception des tables source et de destination.  
  
 Vous pouvez également créer les mêmes index dans la table intermédiaire que dans la partition source. La table intermédiaire contient automatiquement une contrainte basée sur les éléments de la partition source. Cette contrainte est généralement générée à partir de la valeur limite de la partition source.  
  
## <a name="uielement-list"></a>Liste des éléments de l'interface utilisateur  
 **Nom de la table intermédiaire**  
 Créez un nom pour la table intermédiaire ou acceptez le nom par défaut affiché dans la zone d'édition.  
  
 **Basculer la partition**  
 Sélectionnez la partition source que vous souhaitez extraire de la table actuelle.  
  
 **Nouvelle valeur limite**  
 Sélectionnez ou entrez la valeur limite voulue pour la partition dans la table intermédiaire.  
  
 **Groupe de fichiers**  
 Sélectionnez un groupe de fichiers pour la nouvelle table.  
  
 ![Icône de flèche utilisée avec le lien Retour au début](../../analysis-services/instances/media/uparrow16x16.gif "Icône de flèche utilisée avec le lien Retour au début")[Dans cette section](#Top)  
  
##  <a name="OutputOption"></a> Page Sélectionner une option de sortie  
 Utilisez la page **Sélectionner une option de sortie** pour spécifier de quelle manière vous souhaitez apporter les modifications à vos partitions.  
  
### <a name="create-script"></a>Créer un script  
 Une fois qu'il a terminé, l'Assistant crée un script dans l'éditeur de requête afin de modifier des partitions dans la table. Sélectionnez **Créer un script** si vous souhaitez revoir le script, puis l’exécuter manuellement.  
  
 **Générer un script sur fichier**  
 Génère le script sur un fichier .sql. Spécifiez **Unicode** ou **Texte ANSI**. Pour indiquer le nom et l’emplacement du fichier, cliquez sur **Parcourir**.  
  
 **Générer un script sur le Presse-papiers**  
 Enregistrez le script dans le Presse-papiers.  
  
 **Générer un script dans une nouvelle fenêtre de requête**  
 Générez le script dans une fenêtre de l'éditeur de requêtes. Si aucune fenêtre d'éditeur n'est ouverte, une nouvelle fenêtre s'ouvre comme cible du script.  
  
### <a name="run-immediately"></a>Exécuter immédiatement  
 **Run immediately**  
 Indique à l’Assistant de terminer les modifications de partitions lorsque vous cliquez sur **Suivant** ou **Terminer**.  
  
### <a name="schedule"></a>Planifier  
 Sélectionnez cette option pour modifier les partitions de table à des date et heure planifiées.  
  
 **Modifier la planification**  
 Ouvre la boîte de dialogue **Nouvelle planification du travail** , dans laquelle vous pouvez sélectionner, modifier ou consulter les propriétés du travail planifié.  
  
 ![Icône de flèche utilisée avec le lien Retour au début](../../analysis-services/instances/media/uparrow16x16.gif "Icône de flèche utilisée avec le lien Retour au début")[Dans cette section](#Top)  
  
##  <a name="NewJob"></a> Page Nouvelle planification du travail  
 Utilisez la page **Nouvelle planification du travail** pour afficher et modifier les propriétés de la planification.  
  
### <a name="options"></a>Options  
 Sélectionnez le type de planification qui vous intéresse pour le travail de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 **Nom**  
 Tapez un nouveau nom pour la planification.  
  
 **Travaux planifiés**  
 Indique les travaux existants qui utilisent la planification.  
  
 **Type de planification**  
 Permet de sélectionner le type de planification.  
  
 **Activé**  
 Permet d'activer ou de désactiver la planification.  
  
### <a name="recurring-schedule-types-options"></a>Options des types de planification périodique  
 Permet de sélectionner la fréquence du travail planifié.  
  
 **Périodicité**  
 Permet de sélectionner l'intervalle après lequel la planification se répète.  
  
 **Répéter à chaque**  
 Permet de sélectionner le nombre de jours ou de semaines entre deux occurrences de la planification. Cette option n'est pas disponible pour les planifications mensuelles.  
  
 **Lundi**  
 Exécute le travail le lundi. Disponible uniquement pour les planifications hebdomadaires.  
  
 **Mardi**  
 Exécute le travail le mardi. Disponible uniquement pour les planifications hebdomadaires.  
  
 **Mercredi**  
 Exécute le travail le mercredi. Disponible uniquement pour les planifications hebdomadaires.  
  
 **Jeudi**  
 Exécute le travail le jeudi. Disponible uniquement pour les planifications hebdomadaires.  
  
 **Vendredi**  
 Exécute le travail le vendredi. Disponible uniquement pour les planifications hebdomadaires.  
  
 **Samedi**  
 Exécute le travail le samedi. Disponible uniquement pour les planifications hebdomadaires.  
  
 **Dimanche**  
 Exécute le travail le dimanche. Disponible uniquement pour les planifications hebdomadaires.  
  
 **Jour**  
 Sélectionnez le jour du mois pour la planification. Disponible uniquement pour les planifications mensuelles.  
  
 **de chaque**  
 Sélectionnez le nombre de mois entre deux occurrences de la planification. Disponible uniquement pour les planifications mensuelles.  
  
 **Le**  
 Spécifiez une planification pour un jour de la semaine spécifique, dans une semaine spécifique dans le mois. Disponible uniquement pour les planifications mensuelles.  
  
 **Une fois à**  
 Définit l'heure à laquelle le travail est exécuté chaque jour.  
  
 **Toutes les**  
 Définit le nombre d'heures ou de minutes entre chaque exécution du travail.  
  
 **Date de début**  
 Définit la date à laquelle la planification est effective.  
  
 **Date de fin**  
 Définit la date à laquelle la planification cessera d'être appliquée.  
  
 **Aucune date de fin**  
 Spécifie que la planification doit être appliquée indéfiniment.  
  
### <a name="one-time-schedule-types-options"></a>Options des types de planification unique  
 Si vous planifiez un travail pour une exécution unique, vous devez sélectionner des date et heure situées dans le futur.  
  
 **Date**  
 Sélectionnez la date d'exécution du travail.  
  
 **Time**  
 Sélectionnez l'heure d'exécution du travail.  
  
 ![Icône de flèche utilisée avec le lien Retour au début](../../analysis-services/instances/media/uparrow16x16.gif "Icône de flèche utilisée avec le lien Retour au début")[Dans cette section](#Top)  
  
##  <a name="Summary"></a> Page Résumé  
 Utilisez la page **Résumé** pour examiner les options que vous avez sélectionnées sur les pages précédentes.  
  
## <a name="uielement-list"></a>Liste des éléments de l'interface utilisateur  
 **Vérifier vos sélections**  
 Affiche les choix que vous avez effectués pour chaque page de l'Assistant. Cliquez sur un nœud pour développer et afficher les options que vous avez sélectionnées précédemment.  
  
 ![Icône de flèche utilisée avec le lien Retour au début](../../analysis-services/instances/media/uparrow16x16.gif "Icône de flèche utilisée avec le lien Retour au début")[Dans cette section](#Top)  
  
##  <a name="Progress"></a> Page Progression  
 Utilisez la page **Progression** pour surveiller les informations d’état sur les actions de l’ **Assistant Gestion de partition**. Selon les options sélectionnées dans l’Assistant, la page **Progression** peut contenir une ou plusieurs actions. La zone supérieure affiche l'état global de l'Assistant et le nombre des messages d'état, d'erreur et d'avertissement qu'il a reçus.  
  
### <a name="options"></a>Options  
 **Détails**  
 Indique l'action, l'état et tous les messages retournés suite à l'action entreprise par l'Assistant.  
  
 **Action**  
 Indique le type et le nom de chaque action.  
  
 **État**  
 Indique si l’action de l’Assistant dans son ensemble a retourné la valeur **Réussite** ou **Échec**.  
  
 **Message**  
 Indique les messages d'erreur ou d'avertissement retournés par le processus.  
  
 **Arrêter**  
 Arrête l'action de l'Assistant.  
  
 **Rapport**  
 Crée un rapport qui contient les résultats de l’ **Assistant Gestion de partition**. Les options sont :  
  
-   **Afficher le rapport**  
  
-   **Enregistrer le rapport dans un fichier**  
  
-   **Copier le rapport dans le Presse-papiers**  
  
-   **Envoyer le rapport sous forme de courrier électronique**  
  
 **Afficher le rapport**  
 Ouvre la boîte de dialogue **Afficher le rapport** . Cette boîte de dialogue contient un rapport au format texte sur la progression de l’ **Assistant Gestion de partition**.  
  
 **Fermer**  
 Ferme l'Assistant.  
  
 ![Icône de flèche utilisée avec le lien Retour au début](../../analysis-services/instances/media/uparrow16x16.gif "Icône de flèche utilisée avec le lien Retour au début")[Dans cette section](#Top)  
  
## <a name="see-also"></a> Voir aussi  
 [Tables et index partitionnés](../../relational-databases/partitions/partitioned-tables-and-indexes.md)  
  
  
