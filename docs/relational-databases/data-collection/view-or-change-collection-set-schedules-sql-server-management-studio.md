---
title: Afficher ou modifier des planifications de jeu d’éléments de collecte (SQL Server Management Studio) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.suite: sql
ms.technology: supportability
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.swb.dc.collectionsetprop.uploads.f1
- sql13.swb.dc.collectionsetprop.description.f1
- sql13.swb.dc.collectionsetprop.general.f1
helpviewer_keywords:
- collection sets [SQL Server], changing schedules
- schedules [SQL Server], changing collection set
- collection sets [SQL Server], viewing schedules
- schedules [SQL Server], viewing collection set
ms.assetid: 26336c98-78c5-414f-8d6a-574fc3af60c4
caps.latest.revision: 26
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 3e0633f95e2179cacb62c811365878485be61d3a
ms.sourcegitcommit: f1caaa156db2b16e817e0a3884394e7b30fb642f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/04/2018
---
# <a name="view-or-change-collection-set-schedules-sql-server-management-studio"></a>Afficher ou modifier des planifications de jeu d'éléments de collecte (SQL Server Management Studio)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Vous pouvez afficher ou modifier des planifications de jeu d'éléments de collecte à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
 Le mode de collecte, avec ou sans mise en cache, détermine la manière dont vous pouvez apporter des modifications à une planification. Le mode avec mise en cache utilise des planifications distinctes pour la collecte et le téléchargement. Le mode sans mise en cache utilise la même planification pour la collecte et le téléchargement. Le type de mode de collecte pour chacun des jeux d’éléments de collecte de données système est comme suit :  
  
-   **Utilisation du disque** utilise le mode de collecte sans mise en cache.  
  
-   **Statistiques de requête** utilise le mode de collecte avec mise en cache.  
  
-   **Activité du serveur** utilise le mode de collecte avec mise en cache.  
  
### <a name="to-view-collection-set-schedules"></a>Pour afficher des planifications de jeu d'éléments de collecte  
  
1.  Dans l'Explorateur d'objets, développez le nœud **Gestion** et développez **Collecte de données**, puis **Jeux d'éléments de collecte de données système**.  
  
2.  Cliquez avec le bouton droit sur un nom de jeu d’éléments de collecte, puis sélectionnez **Propriétés** pour ouvrir la boîte de dialogue [Propriétés du jeu d’éléments de collecte de données](#CollectionSet) .  
  
### <a name="to-change-the-schedules-for-a-cached-mode-collection-set"></a>Pour modifier les planifications d'un jeu d'éléments de collecte en mode avec mise en cache  
  
1.  Dans l'Explorateur d'objets, développez le nœud **Gestion** et développez **Collecte de données**, puis **Jeux d'éléments de collecte de données système**.  
  
2.  Cliquez avec le bouton droit sur un jeu d’éléments de collecte qui utilise le mode avec mise en cache, tel que **Statistiques sur les requêtes**, puis sélectionnez **Propriétés** pour ouvrir la boîte de dialogue [Propriétés du jeu d’éléments de collecte de données](#CollectionSet) .  
  
3.  Vous pouvez modifier la fréquence de collecte sur la page **Général** . Pour cela, procédez comme suit :  
  
    1.  Dans le volet d’informations, double-cliquez sur le nombre qui s’affiche pour la colonne **Fréquence de collecte (s)** de la table **Éléments de collecte** .  
  
    2.  Pour augmenter ou diminuer la fréquence de collecte, tapez un nombre inférieur ou supérieur, puis appuyez sur Entrée pour stocker la nouvelle valeur.  
  
4.  Pour modifier la planification de téléchargement existante du jeu d'éléments de collecte, procédez comme suit :  
  
    1.  Cliquez sur la page **Téléchargements** .  
  
    2.  Dans le volet d'informations, cliquez sur **Choisir**.  
  
         La boîte de dialogue **Choisir une planification pour le travail** s'affiche. Les planifications disponibles apparaissent sous la forme d'une table.  
  
    3.  Cliquez sur la ligne correspondant à la planification qui vous intéresse. Par exemple, pour modifier la planification de façon à définir un intervalle de cinq minutes, cliquez sur la ligne où le nom de planification est **CollectorSchedule_Every_5min**.  
  
        > [!NOTE]  
        >  Vous pouvez afficher et modifier les propriétés de la planification sélectionnée en cliquant sur **Propriétés** pour ouvrir la boîte de dialogue **Propriétés de la planification du travail** applicable à la planification. Vous pouvez utiliser cette boîte de dialogue pour modifier des informations de planification telles que la fréquence.  
        >   
        >  En guise d'alternative à la modification d'une planification existante, vous pouvez créer une planification de téléchargement en cliquant sur **Nouveau** dans la page **Téléchargements** . Cette action ouvre la boîte de dialogue **Nouvelle planification du travail** , que vous pouvez utiliser pour créer une planification personnalisée.  
  
    4.  Lorsque vous avez fini de configurer la planification, cliquez sur **OK**.  
  
         Les modifications que vous avez apportées apparaissent sur la page **Téléchargements** .  
  
5.  Cliquez sur **OK** pour enregistrer les modifications de fréquence de collecte et de planification des téléchargements, ainsi que pour fermer la boîte de dialogue **Propriétés du jeu d'éléments de collecte de données** .  
  
### <a name="to-change-the-schedule-for-a-non-cached-mode-collection-set"></a>Pour modifier la planification pour un jeu d'éléments de collecte en mode sans mise en cache  
  
1.  Dans l'Explorateur d'objets, développez le nœud **Gestion** et développez **Collecte de données**, puis **Jeux d'éléments de collecte de données système**.  
  
2.  Cliquez avec le bouton droit sur un jeu d’éléments de collecte qui utilise le mode sans mise en cache, tel que **Utilisation du disque**, puis sélectionnez **Propriétés** pour ouvrir la boîte de dialogue [Propriétés du jeu d’éléments de collecte de données](#CollectionSet) .  
  
     La boîte de dialogue **Propriétés du jeu d'éléments de collecte de données** affiche une vue avec pages des propriétés du jeu d'éléments de collecte.  
  
3.  Pour modifier la planification du jeu d'éléments de collecte, cliquez sur **Choisir**.  
  
     La boîte de dialogue **Choisir une planification pour le travail** s'affiche. Les planifications disponibles se présentent sous la forme d'une table.  
  
4.  Cliquez sur la ligne correspondant à la planification qui vous intéresse. Par exemple, pour modifier la planification de façon à définir un intervalle de cinq minutes, cliquez sur la ligne où le nom de planification est **CollectorSchedule_Every_5min**.  
  
    > [!NOTE]  
    >  Vous pouvez afficher et modifier les propriétés de la planification sélectionnée en cliquant sur **Propriétés** pour ouvrir la boîte de dialogue **Propriétés de la planification du travail** applicable à la planification. Vous pouvez utiliser cette boîte de dialogue pour modifier des informations de planification telles que la fréquence.  
    >   
    >  En guise d'alternative à la modification d'une planification existante, vous pouvez créer une planification de collecte et de téléchargement en cliquant sur **Nouveau** dans la page **Général** . Cette action ouvre la boîte de dialogue **Nouvelle planification du travail** .  
  
5.  Lorsque vous avez fini de configurer la planification, cliquez sur **OK**.  
  
6.  Cliquez sur **OK** pour enregistrer les modifications et fermer la boîte de dialogue **Propriétés du jeu d'éléments de collecte de données** .  
  
####  <a name="CollectionSet"></a> Boîte de dialogue Propriétés du jeu d'éléments de collecte de données  
 **Page Général**  
  
 Utilisez cette page pour configurer le mode de collecte et de téléchargement des données, des planifications, ainsi que des périodes de rétention des données dans l'entrepôt de données de gestion. Cette page fournit également des informations sur les jeux d'éléments de collecte, tels que les types de collecteurs et les fréquences de collecte, et les paramètres d'entrée utilisés pour un jeu d'éléments de collecte.  
  
 **Nom**  
 Affiche le nom du jeu d'éléments de collecte auquel cette page fait référence.  
  
 **Collecte et téléchargement de données**  
 Spécifie la façon dont les données sont collectées et téléchargées dans l'entrepôt de données de gestion. Choisissez l'une des options suivantes.  
  
|||  
|-|-|  
|**Aucune mise en cache. Collecte et chargement de données sur la même planification.**|Lorsque vous sélectionnez cette option, spécifiez l'un des éléments suivants :<br /><br /> **Planification**. Les données sont collectées et téléchargées selon une planification. Cliquez sur **Choisir** pour faire votre sélection dans une liste prédéfinie de planifications ou sur **Nouvelle** pour créer une planification.<br /><br /> **À la demande**. Les données sont collectées et téléchargées à la demande.|  
|**Mise en cache. Collecter les données et les mettre en cache selon des fréquences de collecte. Charger les données mises en cache sur une planification distincte.**|Les données sont collectées et mises en cache à une fréquence de collecte spécifiée. Les données collectées sont téléchargées selon une planification séparée.|  
  
 **Éléments de collecte**  
 Affiche les éléments de collecte dans le jeu d'éléments de collecte. Les informations suivantes sont fournies pour chaque élément de collecte :  
  
-   **Nom**  
  
-   **Type de collecteur**  
  
-   **Fréquence de collecte** (s). Ce champ est modifiable si **Collecte et téléchargement de données** est configurée pour la mise en cache. Double-cliquez sur cette cellule pour définir la fréquence de collecte.  
  
 **Paramètres d'entrée**  
 Affiche les paramètres d'entrée utilisés pour le jeu d'éléments de collecte.  
  
 **Exécuter en tant que**  
 Spécifie le compte utilisé pour exécuter le jeu d'éléments de collecte. Par défaut, il s'agit du compte de service Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Si des comptes proxy sont définis, cette liste inclut les noms des comptes proxy disponibles.  
  
 **Spécifier la durée pendant laquelle les données doivent être conservées dans l'entrepôt de données de gestion.**  
 Spécifie la période de rétention des données collectées. Choisissez l'une des options suivantes.  
  
|||  
|-|-|  
|**Conserver les données pendant**|Cette option est sélectionnée par défaut, et la période de rétention par défaut est de 14 jours.|  
|**Conserver les données indéfiniment**|Il n'y a aucune limite sur la période de rétention des données.|  
  
 **Page Téléchargements**  
  
 Utilisez cette page pour configurer la planification du téléchargement pour les données collectées par ce jeu d'éléments de collecte.  
  
> [!NOTE]  
>  Cet onglet est activé uniquement si l’option **Mises en cache** est configurée pour **Collecte et téléchargement de données**.  
  
 **Server**  
 Affiche le nom du serveur qui héberge l'entrepôt de données de gestion. Pour plus d’informations, consultez [Configurer l’entrepôt de données de gestion &#40;SQL Server Management Studio&#41;](../../relational-databases/data-collection/configure-the-management-data-warehouse-sql-server-management-studio.md).  
  
 **Entrepôt de données de gestion**  
 Affiche le nom de l'entrepôt de données de gestion. Pour plus d’informations, consultez [Configurer l’entrepôt de données de gestion &#40;SQL Server Management Studio&#41;](../../relational-databases/data-collection/configure-the-management-data-warehouse-sql-server-management-studio.md).  
  
 **Dernier téléchargement**  
 Affiche des informations sur la date et l'heure du dernier téléchargement effectué pour le jeu d'éléments de collecte.  
  
 **Planification de téléchargement**  
 Spécifie la planification du téléchargement pour le jeu d'éléments de collecte. Si cette option est activée, cliquez sur **Choisir** pour faire votre sélection dans une liste prédéfinie de planifications ou sur **Nouvelle** pour créer une planification.  
  
 **Page Description**  
  
 Utilisez cette page pour consulter une description du jeu d'éléments de collecte auquel cette page de propriétés fait référence.  
  
## <a name="see-also"></a> Voir aussi  
 [Gérer la collecte de données](../../relational-databases/data-collection/manage-data-collection.md)   
 [Collecte de données](../../relational-databases/data-collection/data-collection.md)  
  
  
