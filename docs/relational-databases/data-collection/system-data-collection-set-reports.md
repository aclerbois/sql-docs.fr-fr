---
title: Rapports de jeux d’éléments de collecte de données système | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: supportability
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- data collector [SQL Server], server activity
- server activity [SQL Server]
- reports [SQL Server], data collection
- reports [SQL Server], disk usage
- collection sets [SQL Server], reports
- data collector [SQL Server], reports
- reports [SQL Server], query statistics
- query statistics reports [SQL Server]
- disk usage reports [SQL Server]
ms.assetid: 0b126b8d-4fe7-443d-8a9a-c266350181e5
caps.latest.revision: 24
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 5c859efbcfd431ee2a88f25746debb85ff21746e
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="system-data-collection-set-reports"></a>Rapports de jeux d'éléments de collecte de données système
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Le collecteur de données fournit un rapport historique pour chaque jeu d'éléments de collecte de données système. Chacun des rapports suivants utilise des données stockées dans l'entrepôt de données de gestion :  
  
-   [Résumé sur l'utilisation du disque](#Disk)  
  
-   [Historique des statistiques sur les requêtes](#Query)  
  
-   [Historique de l'activité du serveur](#Server)  
  
 Vous pouvez utiliser ces rapports pour obtenir des informations afin de contrôler la capacité système et de résoudre les problèmes de performances système.  
  
##  <a name="Disk"></a> Rapport Résumé sur l'utilisation du disque  
 Le rapport Résumé sur l'utilisation du disque contient des données sur l'utilisation de l'espace disque pour toutes les bases de données dans l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Les données fournies dans ces rapports sont obtenues à l’aide du jeu d’éléments de collecte Utilisation du disque, qui utilise le type de collecteur Requête T-SQL générique.  
  
 Vous pouvez accéder au rapport Résumé sur l'utilisation du disque à partir de l'Explorateur d'objets. Pour afficher le rapport, développez le dossier **Gestion** , cliquez avec le bouton droit sur **Collecte de données**, pointez sur **Rapports**, sur **Entrepôt de données de gestion**, puis cliquez sur **Résumé sur l’utilisation du disque**. Pour plus d’informations, consultez [Afficher un rapport de jeu d’éléments de collecte &#40;SQL Server Management Studio&#41;](../../relational-databases/data-collection/view-a-collection-set-report-sql-server-management-studio.md).  
  
### <a name="disk-usage-collection-set-report"></a>Rapport Jeu d'éléments de collecte Utilisation du disque  
 Le rapport Jeu d'éléments de collecte Utilisation du disque fournit une vue d'ensemble de l'espace disque utilisé pour toutes les bases de données dans l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]et les tendances de croissance pour les fichiers de données et les fichiers journaux de chacune de ces bases de données.  
  
-   La table de résumé affiche la taille de départ (en mégaoctets) et la taille actuelle de toutes les bases de données installées sur le serveur et contrôlées par le collecteur de données.  
  
-   Les informations relatives à la tendance et à la croissance moyenne sont représentées sous forme graphique et numérique pour les données et les fichiers journaux.  
  
#### <a name="disk-usage-collection-set---database-databasename-subreport"></a>Sous-rapport Jeu d'éléments de collecte Utilisation du disque - Base de données : <nom_base_de_données>  
 Le sous-rapport Jeu d’éléments de collecte Utilisation du disque - Base de données : *<nom_base_de_données>* s’affiche quand vous cliquez sur une courbe de tendance pour une base de données ou un fichier journal spécifique dans la table de résumé du rapport Jeu d’éléments de collecte Utilisation du disque. Ce rapport fournit une représentation graphique des tendances de croissance concernant l'utilisation de l'espace disque sur la période du rapport. L'utilisation du disque est organisée et indiquée par espace utilisé, espace de données, espace non alloué et espace d'index pour les fichiers de données et par espace utilisé et espace inutilisé pour les fichiers journaux.  
  
 La table située au-dessous du graphique répertorie les heures de collecte de données et les données d'utilisation correspondantes.  
  
#### <a name="disk-usage-for-database-databasename-subreport"></a>Sous-rapport Utilisation du disque pour la base de données : <nom_base_de_données>  
 Le sous-rapport **Utilisation du disque pour la base de données :***<nom_base_de_données>* s’affiche quand vous cliquez sur un nom de base de données dans la table de résumé du rapport Jeu d’éléments de collecte Utilisation du disque. Ce rapport fournit une répartition numérique et graphique de l'utilisation de l'espace par les fichiers de données et les fichiers journaux de transactions de la base de données. L'utilisation de l'espace pour les fichiers de données est catégorisée en tant que pourcentage alloué aux pages d'index, à l'espace non alloué, aux pages de données et à l'espace inutilisé. Ces catégories sont définies comme suit :  
  
|Catégorie|Définition|  
|--------------|----------------|  
|Index|Quantité d'espace disque utilisée pour contenir les pages d'index.|  
|Non alloué|Quantité d'espace disque disponible pour la base de données mais non encore allouée à un objet.|  
|data|Quantité d'espace disque utilisée par les pages de données.|  
|Inutilisé|Quantité d'espace disque allouée à un ou plusieurs objets, mais pas encore utilisée.|  
  
 L'utilisation de l'espace pour le fichier journal de transactions est catégorisée en tant qu'espace utilisé et espace inutilisé.  
  
 Les événements autogrow et autoshrink sont signalés pour les fichiers de données et les fichiers journaux s'ils se sont produits dans la base de données. Le rapport affiche l'heure de début et la durée de chaque événement ainsi que la modification qui en résulte dans la taille du fichier.  
  
 L'espace disque utilisé par chaque fichier de données dans la base de données est signalé. L'espace signalé comme Espace réservé est la quantité d'espace utilisée plus l'espace alloué au fichier mais pas encore utilisé. L'espace signalé comme Espace utilisé est l'espace réel actuellement utilisé par le fichier, à l'exception de l'espace alloué.  
  
##  <a name="Query"></a> Rapport Historique des statistiques sur les requêtes  
 Le rapport Historique des statistiques sur les requêtes contient des statistiques sur l'exécution des requêtes. Les données utilisées dans ce rapport sont obtenues à l'aide du jeu d'éléments de collecte Statistiques sur les requêtes, qui utilise le type de collecteur Activité des requêtes.  
  
 Vous pouvez accéder au rapport Historique des statistiques sur les requêtes à partir de l'Explorateur d'objets. Pour afficher le rapport, développez le dossier **Gestion** , cliquez avec le bouton droit sur **Collecte de données**, pointez sur **Rapports**, sur **Entrepôt de données de gestion**, puis cliquez sur **Historique des statistiques sur les requêtes**. Pour plus d’informations, consultez [Afficher un rapport de jeu d’éléments de collecte &#40;SQL Server Management Studio&#41;](../../relational-databases/data-collection/view-a-collection-set-report-sql-server-management-studio.md).  
  
### <a name="selecting-data-to-include-in-the-report"></a>Sélection des données à inclure dans le rapport  
 Le rapport inclut des statistiques sur l'exécution des requêtes pour l'intégralité de la période de collecte de données. Vous pouvez naviguer dans la chronologie de la collecte de données de deux manières pour sélectionner un segment de données à afficher.  
  
 **Contrôle de chronologie et boutons de navigation**  
  
 Utilisez le contrôle de chronologie et les boutons de navigation pour parcourir la chronologie ou pour sélectionner une plage de dates. Les boutons fléchés permettent de faire défiler vers la droite et vers la gauche de manière à avancer ou à reculer dans la chronologie. Par défaut, les flèches permettent de parcourir la chronologie par incréments de 4 heures. À l'aide des boutons de loupe, vous pouvez développer ou contracter cet incrément d'heure sur l'une des valeurs suivantes : 15 minutes, 1 heure, 4 heures, 12 heures ou 24 heures. La plage temporelle actuellement sélectionnée est indiquée par la partie en surbrillance de la chronologie et affichée dans le texte au-dessous de celle-ci. Ces valeurs, ainsi que les données du rapport, sont mises à jour chaque fois que vous cliquez sur la chronologie ou que vous utilisez les boutons de navigation.  
  
 **Bouton Calendrier**  
  
 Utilisez le bouton Calendrier pour spécifier la date de début, l'heure de début et la durée des données pour lesquelles vous voulez créer un rapport.  
  
#### <a name="query-statistics-history-report"></a>Rapport Historique des statistiques sur les requêtes  
 Le graphique Requêtes principales par temps processeur total montre la dépense relative de chaque requête pour la plage temporelle sélectionnée par rapport à l'utilisation totale du processeur. Pour afficher une vue différente des dépenses de requêtes relatives, cliquez sur l’un des liens hypertexte situés sous le graphique : **Durée**, **Total E/S**, **Lectures physiques**ou **Écritures logiques**.  
  
 La table située au-dessous du graphique fournit des données de requête supplémentaires. Elle répertorie le texte de chaque requête représentée dans le graphique avec des informations statistiques détaillées. Notez que les barres de graphique sont des liens actifs, comme le sont toutes les requêtes répertoriées dans la table. Un clic sur un lien actif ouvre le sous-rapport Détails de la requête.  
  
#### <a name="query-details-subreport"></a>Sous-rapport Détails de la requête  
 Le sous-rapport Détails de la requête fournit l'intégralité du texte de la requête. Un lien hypertexte **Modifier le texte de la requête** est situé juste à côté de la requête. Vous pouvez cliquer sur ce lien pour ouvrir la requête dans l'Éditeur de requête. La table située sous la fenêtre de requête fournit des statistiques sur l'exécution de la requête, telles que la durée moyenne par exécution de requête.  
  
 Un graphique de plans de requête et la durée moyenne par exécution sont affichés. Pour afficher une vue différente du coût de plan de requête relatif, cliquez sur l'un des liens hypertexte affichés sous le graphique : **Durée**, **Lectures physiques**ou **Écritures logiques**. La courbe de graphique est active et vous pouvez cliquer sur un point quelconque pour ouvrir le sous-rapport Détails du plan de requête.  
  
 La table située au-dessous du graphique montre les 10 principaux plans de requête, identifiés en fonction de l'utilisation de l'UC par exécution. Chaque nombre figurant dans la colonne **Plan numéro** est un lien actif sur lequel vous pouvez cliquer pour ouvrir le sous-rapport Détails du plan de requête.  
  
#### <a name="query-plan-details-subreport"></a>Sous-rapport Détails du plan de requête  
 Ce rapport affiche les informations relatives à un plan de requête. En plus de vous permettre de modifier la requête et d'afficher les statistiques d'exécution, le rapport fournit des informations détaillées à propos du plan de requête. Le lien hypertexte **Afficher le plan graphique d'exécution de la requête** ouvre une représentation graphique du plan d'exécution de la requête actuelle.  
  
## <a name="server-activity-history-report"></a>Rapport Historique de l'activité du serveur  
 Le rapport Historique de l'activité du serveur contient des données sur la consommation de ressources et sur l'activité du serveur pour le serveur et pour l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Les données fournies dans ce rapport sont collectées par le jeu d’éléments de collecte Activité du serveur, qui utilise le type de collecteur Requête T-SQL générique et le type de collecteur Compteurs de performance.  
  
 Vous pouvez accéder au rapport Historique de l'activité du serveur à partir de l'Explorateur d'objets. Pour afficher le rapport, développez le dossier **Gestion** , cliquez avec le bouton droit sur **Collecte de données**, pointez sur **Rapports**, sur **Entrepôt de données de gestion**, puis cliquez sur **Historique de l’activité du serveur**. Pour plus d’informations, consultez [Afficher un rapport de jeu d’éléments de collecte &#40;SQL Server Management Studio&#41;](../../relational-databases/data-collection/view-a-collection-set-report-sql-server-management-studio.md).  
  
### <a name="selecting-data-to-include-in-the-report"></a>Sélection des données à inclure dans le rapport  
 Le rapport inclut l'activité du serveur pour l'intégralité de la période de collecte de données. Vous pouvez naviguer dans la chronologie de la collecte de données de deux manières pour sélectionner un segment de données à afficher.  
  
 **Contrôle de chronologie et boutons de navigation**  
  
 Utilisez le contrôle de chronologie et les boutons de navigation pour parcourir la chronologie ou pour sélectionner une plage de dates. Les boutons fléchés permettent de faire défiler vers la droite et vers la gauche de manière à avancer ou à reculer dans la chronologie. Par défaut, les flèches permettent de parcourir la chronologie par incréments de 4 heures. À l'aide des boutons de loupe, vous pouvez développer ou contracter cet incrément d'heure sur l'une des valeurs suivantes : 15 minutes, 1 heure, 4 heures, 12 heures ou 24 heures. La plage temporelle actuellement sélectionnée est indiquée par la partie en surbrillance de la chronologie et affichée dans le texte au-dessous de celle-ci. Ces valeurs, ainsi que les données du rapport, sont mises à jour chaque fois que vous cliquez sur la chronologie ou que vous utilisez les boutons de navigation.  
  
 **Bouton Calendrier**  
  
 Utilisez le bouton Calendrier pour spécifier la date de début, l'heure de début et la durée des données pour lesquelles vous voulez créer un rapport.  
  
###  <a name="Server"></a> Rapport Historique de l'activité du serveur  
 Le rapport Historique de l'activité du serveur montre la vue initiale de l'activité du serveur pour une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et le système d'exploitation hôte.  
  
 La table suivante décrit les graphiques qui tracent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et l'activité du système dans le rapport et dans les sous-rapports détaillés accessibles via les graphiques.  
  
|Graphique|Description du rapport|  
|-----------|------------------------|  
|% UC|Ces sous-rapports sont accessibles en cliquant sur un point des courbes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou du système dans le graphique % UC.<br /><br /> **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** : le rapport Historique des statistiques sur les requêtes fournit un graphique des requêtes les plus coûteuses dans l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Une table située au-dessous du graphique répertorie les requêtes et inclut des données statistiques pour chaque requête. Vous pouvez cliquer sur une requête pour obtenir des détails supplémentaires.<br /><br /> **Système**: le rapport Utilisation de l’UC du système fournit un graphique du % temps UC par processeur et des données statistiques pour chaque processus, sous la forme d’un tableau.|  
|Utilisation de la mémoire|Ces sous-rapports sont accessibles en cliquant sur un point des courbes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou du système dans le graphique Utilisation de la mémoire.<br /><br /> **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** : le rapport [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Utilisation de la mémoire fournit des graphiques sur l’utilisation de la mémoire du processus [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , les compteurs mémoire, la consommation de la mémoire interne par type et une table qui contient des données sur l’utilisation moyenne de la mémoire par type de composant.<br /><br /> **Système**: le rapport Utilisation de la mémoire système fournit des graphiques sur l’utilisation de la mémoire, les taux d’accès aux caches et aux pages, ainsi qu’une table qui fournit des informations sur la plage de travail et les octets privés pour chaque processus.|  
|Utilisation des E/S disque|Ces sous-rapports sont accessibles en cliquant sur un point des courbes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou du système dans le graphique Utilisation des E/S disque.<br /><br /> **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** : le rapport [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Utilisation des E/S disque fournit des graphiques sur le temps de réponse disque et le taux de transfert disque. Une table supplémentaire fournit des statistiques sur les fichiers virtuels par disque, base de données et fichier.<br /><br /> **Système**: le rapport Utilisation du disque système fournit des graphiques sur le temps de réponse disque, la longueur moyenne de la file d’attente du disque, le taux de transfert disque, ainsi qu’une table qui contient des informations sur les écritures et les lectures d’E/S pour chaque processus.|  
|Utilisation du réseau|Aucun rapport supplémentaire n'est disponible.|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Attentes|Le graphique Attentes [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] affiche les attentes rencontrées par les threads exécutés par catégorie d'attente. Vous pouvez accéder à un rapport détaillé en cliquant sur un segment du graphique. En plus de fournir des statistiques sur les attentes [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour une plage de temps plus étroite, ce rapport fournit des informations à propos des catégories d'attente sous forme de tableau. Pour chaque catégorie, telle que l'UC et ses sous-catégories, le tableau indique le nombre d'attentes, le temps d'attente et le pourcentage total de temps d'attente.|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Activité|Les différents aspects de l'activité de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont accessibles à partir du graphique Activité de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Les rapports que vous pouvez obtenir en cliquant sur un point sur la courbe de graphique de Compilations SQL/s sont les suivants :<br /><br /> <br /><br /> Connexions et sessions<br /><br /> Demandes<br /><br /> Taux d'accès au plan du travail<br /><br /> Caractéristiques tempdb|  
  
## <a name="see-also"></a> Voir aussi  
 [Collecte de données](../../relational-databases/data-collection/data-collection.md)   
 [Afficher un rapport de jeu d’éléments de collecte &#40;SQL Server Management Studio&#41;](../../relational-databases/data-collection/view-a-collection-set-report-sql-server-management-studio.md)  
  
  
