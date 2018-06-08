---
title: Aide (F1) sur l’Explorateur d’utilitaire | Microsoft Docs
ms.custom: ''
ms.date: 08/19/2016
ms.prod: sql
ms.prod_service: database-engine
ms.component: maintenance-plans
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: reference
f1_keywords:
- sql13.swb.ue.navigation.f1
- sql13.SWB.UE.dac.details.F1
- sql13.SQB.UE.dac.details.F1
- utility details
helpviewer_keywords:
- Utility
- management
- data-tier application
ms.assetid: 8697e4a4-4f59-4cda-af71-7de86005bd4a
caps.latest.revision: 8
author: MikeRayMSFT
ms.author: mikeray
manager: craigg
ms.openlocfilehash: 30fc710837c06d61788b330bfe0c603748e3d6a8
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="utility-explorer-f1-help"></a>Aide sur l'Explorateur d'objets accessible via la touche F1
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Les sections suivantes documentent les fonctionnalités de l’utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et les opérations associées.  
  
  ## <a name="utility-dashboard-sql-server-utility"></a>Tableau de bord de l'utilitaire (utilitaire SQL Server)
 Pour consulter des données dans le tableau de bord de l’utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], sélectionnez le nœud supérieur dans l’arborescence de l’Explorateur de l’utilitaire - étiqueté « Utility<nom_UCP\\\(NomOrdinateur\UCP) ». Le tableau de bord inclut les données de synthèse et les données détaillées de toutes les instances managées de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et de toutes les applications de couche Données dans l'utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour actualiser les données du tableau de bord, cliquez avec le bouton droit sur le nœud supérieur de l’arborescence de l’Explorateur de l’utilitaire et sélectionnez **Actualiser**.  
  
 Pour plus d’informations sur la création d’un point de contrôle d’utilitaire, consultez [Créer un point de contrôle de l’utilitaire SQL Server &#40;utilitaire SQL Server&#41;](../../relational-databases/manage/create-a-sql-server-utility-control-point-sql-server-utility.md). Pour plus d’informations sur l’ajout d’une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à l’utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , consultez [Inscrire une instance de SQL Server &#40;utilitaire SQL Server&#41;](../../relational-databases/manage/enroll-an-instance-of-sql-server-sql-server-utility.md).  
 
  
### <a name="uielement-list"></a>Liste des éléments de l'interface utilisateur  
 Intégrité de l'instance gérée  
 L'état d'intégrité des instances gérées de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s'affiche sur le côté gauche du volet de contenu de l'Explorateur de l'utilitaire.  
  
 Les paramètres d'intégrité de l'instance gérée sont les suivants :  
  
-   utilisation du processeur pour l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)];  
  
-   utilisation du fichier de base de données ;  
  
-   utilisation de l'espace du volume de stockage ;  
  
-   utilisation du processeur par l'ordinateur ;  
  
-   L'état de chaque paramètre est divisé en trois catégories :  
  
-   bien utilisé : nombre d'instances gérées de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui ne violent pas les stratégies d'utilisation des ressources ;  
  
-   sous-exploité : nombre de ressources gérées qui violent les stratégies de sous-exploitation des ressources ;  
  
-   surexploité : nombre de ressources gérées qui violent les stratégies de surexploitation des ressources ;  
  
-   aucune donnée disponible : les données ne sont pas non plus disponibles pour les instances gérées de SQL Server parce que l'instance de SQL Server vient d'être inscrite et que la première opération de collecte de données n'est pas terminée, ou parce que l'instance gérée de SQL Server rencontre un problème pour recueillir et télécharger des données vers l'UCP.  
  
 L'état détaillé de chaque paramètre d'intégrité est répertorié dans des curseurs. La fraction située à la droite des curseurs montre combien d'instances gérées se trouvent dans chaque catégorie d'état.  
  
 Pour créer une vue filtrée d'une instance gérée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou une application de la couche Données, cliquez sur le lien pour une catégorie d'utilisation en regard de son curseur dans le tableau de bord de l'utilitaire. Par exemple, si vous cliquez sur **Processeur des instances surexploité** dans le volet **Contenu de l'Explorateur de l'utilitaire** , SSMS crée une vue de liste filtrée des instances gérées de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui ont l'UCP surexploité selon les paramètres de stratégie actuels.  
  
 Remarquez que lorsque vous cliquez sur un lien pour une catégorie d’utilisation, le nœud correspondant dans le volet de navigation Explorateur de l’utilitaire est ajouté avec **(filtré)** - c’est-à-dire, **Instances gérées** est étiqueté **Instances gérées (filtré)**. Pour consulter des paramètres de filtre, cliquez avec le bouton droit sur le nœud dans le volet de navigation et sélectionnez **Filtre**, puis cliquez sur **Paramètres du filtre**. Pour effacer des paramètres de filtre, cliquez avec le bouton droit sur le nœud dans le volet de navigation et sélectionnez **Filtre**, puis cliquez sur **Supprimer le filtre**.  
  
 Pour plus d’informations sur la consultation de l’état d’instances individuelles de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ou pour afficher ou modifier des paramètres de configuration de stratégie, consultez [Détails de l’instance gérée &#40;utilitaire SQL Server&#41;](http://msdn.microsoft.com/library/6e51b7bb-a733-4852-8c33-7f4dbdf931c2).  
  
 Résumé de l'utilitaire  
 Affiche le nombre d'instances managées de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et le nombre d'applications de couche Données gérées par l'utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 État d'intégrité des applications de couche Données  
 L'état d'intégrité des applications de couche Données s'affiche sur le côté droit du volet de contenu de l'Explorateur de l'utilitaire.  
  
 Les paramètres d'état d'intégrité des applications de couche Données sont les suivants :  
  
-   utilisation du processeur pour l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)];  
  
-   utilisation du fichier de base de données ;  
  
-   utilisation de l'espace du volume de stockage ;  
  
-   utilisation du processeur par l'ordinateur ;  
  
 L'état de chaque paramètre est divisé en trois catégories :  
  
-   bien utilisé : nombre d'applications de couche Données qui ne violent pas les stratégies d'utilisation des ressources ;  
  
-   surexploité : nombre d'applications de couche Données qui ne violent pas les stratégies de surexploitation des ressources ;  
  
-   sous-exploité : nombre d'applications de couche Données qui ne violent pas les stratégies de sous-exploitation des ressources ;  
  
-   aucune donnée disponible : les données ne sont pas disponibles pour les applications de couche Données parce que l'instance gérée de SQL Server qui contient l'application de couche Données ne signale pas de données.  
  
 L'état détaillé de chaque paramètre d'intégrité est répertorié dans des curseurs. La fraction située à droite des curseurs montre combien d'applications de couche Données se trouvent dans chaque catégorie d'état. Pour plus d’informations sur la consultation de l’état des instances de la couche Données, ou pour afficher ou modifier des paramètres de configuration de stratégie, consultez [Détails des applications de la couche Données déployées &#40;utilitaire SQL Server&#41;](http://msdn.microsoft.com/library/79c41dd9-abcb-434e-9326-00a341d5c867).  
  
 Historique de l'utilisation de l'utilitaire du stockage  
 L'historique de l'utilisation s'affiche dans un graphique temporel situé au bas du tableau de bord de l'utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Notez que les données temporelles affichent la date et l'heure locales du processeur à l'aide du type de données datetime. Pour plus d’informations, consultez la rubrique [datetime (Transact-SQL)](http://go.microsoft.com/fwlink/?LinkId=164071) . Lorsque vous utilisez le modèle d'objet de l'utilitaire, notez que SSMS utilise le type de données datetimeoffset. Pour plus d’informations, consultez la rubrique [datetimeoffset (Transact-SQL)](http://go.microsoft.com/fwlink/?LinkId=141713) .  
  
 Utilisez les cases d'option situées à gauche de la zone d'affichage pour modifier la période de création de rapports pour le graphique.  
  
 Les options d'intervalle de création de rapport sont les suivants :  
  
-   1 jour, affiché par intervalles de 15 minutes ;  
  
-   1 semaine, affiché par intervalles de 1 jour ;  
  
-   1 mois, affiché par intervalles de 1 semaine ;  
  
-   1 an, affiché par intervalles de 1 mois.  
  
 Une fois que vous avez apporté une modification à l'intervalle de création de rapport, les données sont automatiquement actualisées.  
  
 Utilisation du stockage par l'utilitaire  
 Dans la partie inférieure droite du tableau de bord, le graphique à secteurs de l'utilisation du stockage affiche le rapport entre l'espace utilisé et l'espace libre sur les volumes résidant sur des ordinateurs qui contiennent des instances gérées de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Les données de cet affichage sont actualisées toutes les 15 minutes.  
 
 ## <a name="deployed-data-tier-application-details-sql-server-utility"></a>Détails des applications de la couche Données déployées (utilitaire SQL Server)
  Les informations de la vue Applications de la couche Données déployées de l'Explorateur de l'utilitaire fournissent des données d'utilisation pour les applications de la couche Données, l'historique de l'utilisation du processeur, des détails sur l'utilisation du stockage au niveau du fichier, ainsi que la capacité d'afficher et de mettre à jour des seuils de stratégie. Les seuils de stratégie peuvent être contrôlés au niveau de l'application de la couche Données pour l'utilisation du processeur et pour les fichiers des données de la base de données et les fichiers journaux. Vous pouvez également consulter les détails des propriétés des applications de la couche Données.  
  
### <a name="uielement-list"></a>Liste des éléments de l'interface utilisateur  
 Mode Liste  
 Le mode Liste, dans le volet supérieur, affiche des données concernant des applications de la couche Données. Les icônes d'état d'intégrité fournissent un résumé de l'état de chaque application de la couche Données par catégorie d'utilisation :  
  
-   Coche verte (![](../../relational-databases/manage/media/well-utilized.gif "Correctement exploité")) : nombre d’applications de la couche Données qui ne violent pas les stratégies d’utilisation des ressources. Les ressources sont bien utilisées.  
  
-   Flèche bas verte (![](../../relational-databases/manage/media/utility-down-arrow.gif "Utilitaire, flèche bas")) : les ressources sont sous-exploitées.  
  
-   Flèche haut rouge (![](../../relational-databases/manage/media/utility-up-arrow.gif "Utilitaire, flèche haut")) : les ressources sont surexploitées.  
  
 La séquence des colonnes en mode Liste peut être modifiée en les faisant glisser vers la gauche ou la droite. Les colonnes en mode Liste peuvent être ajoutées ou supprimées en cliquant avec le bouton droit sur leurs en-tête et en les sélectionnant ou désélectionnant. Le menu contextuel offre également des options de tri. Le tri peut également être activé en cliquant en haut d'un nom de colonne.  
  
 Pour accéder aux options du filtre du mode Liste de l’utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , cliquez avec le bouton droit sur le nœud **Applications de la couche Données déployées** dans le volet de navigation de l’Explorateur de l’utilitaire et sélectionnez **Filtre**. Une fois les paramètres de filtrage implémentés, le nœud **Applications de la couche Données déployées** est étiqueté **Applications de la couche Données déployées (filtré)** dans l’Explorateur de l’utilitaire. Pour plus d’informations, consultez [Paramètres de filtre &#40;Explorateur d’objets et Explorateur de l’utilitaire&#41;](http://msdn.microsoft.com/library/4aab04bc-e1ab-4d4b-ab74-b287fc805bc2).  
  
 Par défaut, les colonnes suivantes affichent les informations d'état d'intégrité de chaque application de la couche Données.  
  
-   Nom : le nom de l'application de la couche Données.  
  
-   Processeur de l'application : affiche l'état d'intégrité de l'utilisation du processeur pour cette application de la couche Données. L'état d'intégrité de ce paramètre est déterminé en fonction de la stratégie d'utilisation du processeur définie pour l'application de la couche Données et du paramètre de configuration de la stratégie d'évaluation des ressources volatiles. Pour plus d’informations, consultez [Réduire le bruit dans les stratégies d’utilisation du processeur &#40;utilitaire SQL Server&#41;](../../relational-databases/manage/reduce-noise-in-cpu-utilization-policies-sql-server-utility.md).  
  
     Pour consulter l’historique de l’utilisation du processeur pour cette application de la couche Données ou pour afficher ou modifier les limites de la stratégie, cliquez sur l’onglet **Utilisation du processeur**.  
  
-   Processeur de l'ordinateur : affiche l'état d'intégrité de l'utilisation du processeur de l'ordinateur. L'état d'intégrité de ce paramètre est déterminé en fonction de la stratégie d'utilisation du processeur définie pour l'ordinateur et du paramètre de configuration de la stratégie d'évaluation des ressources volatiles. Pour plus d’informations, consultez [Réduire le bruit dans les stratégies d’utilisation du processeur &#40;utilitaire SQL Server&#41;](../../relational-databases/manage/reduce-noise-in-cpu-utilization-policies-sql-server-utility.md).  
  
     Pour consulter l’historique de l’utilisation du processeur pour cette application de la couche Données ou pour afficher ou modifier les limites de la stratégie, cliquez sur l’onglet **Utilisation du processeur**.  
  
-   Espace de fichier : affiche un résumé des états d'intégrité de l'utilisation de l'espace de fichier pour chaque application de la couche Données.  
  
    -   Coche verte : les états d'intégrité de tous les groupes de fichiers et du groupe de fichier journal ne sont ni surexploités ni sous-exploités.  
  
    -   Flèche bas verte : l'état d'intégrité d'au moins un groupe de fichiers ou du groupe de fichiers journaux est sous-exploité et aucun groupe de fichiers ou groupe de fichiers journaux n'est surexploité.  
  
    -   Flèche haut rouge : l'état d'intégrité d'au moins un groupe de fichiers ou du groupe de fichier journal est surexploité. Notez que si la base de données se trouve dans l'état « urgence », l'état d'intégrité affiche l'espace de fichier journal surexploité.  
  
     Pour afficher ou modifier les limites de la stratégie d’espace de fichier, cliquez sur l’onglet **Utilisation du stockage** .  
  
-   Espace de volume : affiche un résumé des états d'intégrité de l'utilisation de l'espace de volume pour tous les volumes contenant des bases de données qui appartiennent à l'application de la couche Données sélectionnée. Si l'état d'intégrité d'un des volumes est surexploité, l'état d'intégrité de l'espace de volume sera signalé en mode Liste comme étant surexploité. Si l'état d'intégrité d'un des volumes de données est sous-exploité et qu'aucun volume n'est surexploité, l'état d'intégrité de l'espace de volume sera signalé en mode Liste comme étant sous-exploité. Sinon, l'état d'intégrité de l'espace de volume sera signalé en mode Liste comme étant correctement utilisé. Pour afficher ou modifier les limites de la stratégie, cliquez sur l’onglet **Utilisation du stockage** .  
  
-   Type de stratégie : indique si les stratégies « Globales » par défaut ou des stratégies de « Substitution » personnalisées sont appliquées pour l'application de la couche Données.  
  
-   Nom de l'instance : affiche le nom de l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui héberge l'application de la couche Données.  
  
 Autres colonnes pouvant s'afficher à l'aide du menu contextuel dans la zone d'en-tête de colonne du mode Liste :  
  
-   Nom de la base de données  
  
-   Date déployée  
  
-   Digne de confiance : (True ou False)  
  
-   Classement  
  
-   Niveau de compatibilité : (par exemple, Version100)  
  
-   Chiffrement activé : (True ou False)  
  
-   Mode de récupération : (simple, complet ou en utilisant les journaux de transactions)  
  
-   Dernière heure signalée : cette colonne affiche l'heure et la date locales du processeur à l'aide du type de données datetime. Pour plus d’informations, consultez la rubrique [datetime (Transact-SQL)](http://go.microsoft.com/fwlink/?LinkId=164071) . Lorsque vous utilisez le modèle d'objet de l'utilitaire, notez que SSMS utilise le type de données datetimeoffset. Pour plus d’informations, consultez la rubrique [datetimeoffset (Transact-SQL)](http://go.microsoft.com/fwlink/?LinkId=141713) .  
  
 Onglet Utilisation du processeur  
 L'onglet d'utilisation du processeur affiche côte à côte des graphiques de données d'historique pour l'application de la couche Données et l'utilisation du processeur de l'ordinateur.  
  
 Le graphique affiche l'historique de l'utilisation du processeur pour l'intervalle spécifié dans les cases d'option situées sur le côté droit de la zone d'affichage. Pour modifier l'intervalle d'affichage et actualiser le jeu de données, sélectionnez une case d'option sur la liste.  
  
 Les options d'intervalle sont les suivantes :  
  
-   1 jour, affiché par intervalles de 15 minutes ;  
  
-   1 semaine, affiché par intervalles de 1 jour ;  
  
-   1 mois, affiché par intervalles de 1 semaine ;  
  
-   1 an, affiché par intervalles de 1 mois.  
  
 Onglet Utilisation du stockage  
 L'arborescence de l'onglet Utilisation du stockage montre les détails de l'utilisation de stockage pour les fichiers de base de données et les fichiers journaux qui appartiennent à l'application de la couche Données sélectionnée en mode Liste. Notez que les données temporelles affichent la date et l'heure locales du processeur à l'aide du type de données datetime. Pour plus d’informations, consultez la rubrique [datetime (Transact-SQL)](http://go.microsoft.com/fwlink/?LinkId=164071) . Lorsque vous utilisez le modèle d'objet de l'utilitaire, notez que SSMS utilise le type de données datetimeoffset. Pour plus d’informations, consultez la rubrique [datetimeoffset (Transact-SQL)](http://go.microsoft.com/fwlink/?LinkId=141713) .  
  
 L'affichage peut être regroupé par groupe de fichiers ou par volume. Pour utiliser l’arborescence de groupe de fichiers, sélectionnez la case d’option **Groupe de fichiers** dans la sélection **Regrouper les fichiers par :** .  
  
 Les données affichées dans le graphique d'historique de l'utilisation du stockage changent en fonction du nœud sélectionné dans l'arborescence :  
  
-   Sélectionnez le nœud groupe de fichiers pour afficher un graphique de l'espace de fichier utilisé par tous les fichiers de données appartenant au groupe de fichiers sélectionné.  
  
-   Sélectionnez le nœud fichier journal pour afficher un graphique de l'espace de fichier utilisé par tous les fichiers journaux appartenant à la base de données sélectionnée.  
  
-   Sélectionnez le nœud base de données pour afficher un graphique qui additionne l'espace de fichier utilisé par tous les fichiers de données et tous les fichiers journaux qui appartiennent à la base de données sélectionnée.  
  
 Pour consulter l'état de l'utilisation du stockage pour des fichiers individuels, cliquez sur le signe plus en regard du nom d'un fichier dans l'arborescence.  
  
 Les icônes d'état d'intégrité fournissent en un coup d'œil l'état de chaque groupe de fichiers en fonction des seuils de stratégie :  
  
-   Coche verte : l'utilisation de l'espace de fichier d'au moins un fichier de données du groupe de fichiers n'est ni surexploitée ni sous-exploitée.  
  
-   Flèche bas verte : l'utilisation de l'espace de fichier d'au moins un fichier de données du groupe de fichiers est sous-exploitée et aucun fichier du groupe de fichiers n'est surexploité.  
  
-   Flèche haut rouge : l'utilisation de l'espace de fichier de tous les fichiers de données du groupe de fichiers est surexploitée. Notez que si la base de données se trouve dans l'état « urgence », l'état d'intégrité affiche l'espace de fichier journal surexploité.  
  
 Pour consulter les fichiers par volume, sélectionnez la case d’option **Volume** dans la sélection **Regrouper les fichiers par** . Le graphique d'historique de l'utilisation du stockage montre l'espace de fichier utilisé par tous les fichiers de données et tous les fichiers journaux du volume de stockage. Développez l'arborescence pour voir le détail des fichiers des données et des fichiers journaux de la base de données.  
  
 Les icônes d'état servent à fournir l'état de chaque volume :  
  
-   Coche verte : les ressources sont bien utilisées.  
  
-   Flèche bas verte : les ressources sont sous-exploitées.  
  
-   Flèche haut rouge : les ressources sont surexploitées.  
  
 La jauge en regard de chaque nom de fichier sous l'onglet Utilisation du stockage représente la quantité d'espace utilisée par le fichier par rapport à sa capacité effective. La valeur en pourcentage affichée en regard de la jauge est le rapport entre la quantité d'espace utilisée par le fichier et la capacité effective du fichier. Les info-bulles fournissent des données qui servent à calculer le pourcentage de chaque volume et de chaque fichier par rapport à la capacité effective.  
  
 Si le fichier n'est pas configuré pour s'agrandir automatiquement, alors la capacité effective sera la quantité d'espace allouée au fichier. Si le fichier est configuré pour s'agrandir automatiquement, la capacité effective sera la somme de la quantité d'espace actuellement allouée au fichier et de la quantité d'espace libre actuellement disponible sur le volume.  
  
 Les stratégies d'utilisation du stockage peuvent être configurées pour les fichiers de données et pour les fichiers journaux. Pour afficher ou modifier les seuils de la stratégie d’utilisation du stockage pour les fichiers, cliquez sur le lien **Stratégie Fichiers** au bas du volet Utilisation du stockage. Pour afficher ou modifier les seuils de la stratégie d’utilisation du stockage pour un volume de stockage, cliquez sur le lien **Stratégie Volumes** au bas du volet Utilisation du stockage.  
  
 Pour remplacer les seuils de stratégie par défaut, cliquez sur la case d’option pour **Remplacer la stratégie par défaut**, spécifiez des valeurs pour les limites supérieures et inférieures, puis cliquez sur **OK**.  
  
 Pour plus d’informations sur la modification de la tolérance des violations de stratégie, consultez [Réduire le bruit dans les stratégies d’utilisation du processeur &#40;Utilitaire SQL Server&#41;](../../relational-databases/manage/reduce-noise-in-cpu-utilization-policies-sql-server-utility.md).  
  
 Onglet Détails des propriétés  
 Les détails des propriétés répertoriés pour les applications de la couche Données incluent les catégories suivantes :  
  
-   Nom de la base de données  
  
-   Date déployée  
  
-   Digne de confiance : (True ou False)  
  
-   Classement  
  
-   Niveau de compatibilité : (par exemple, Version100)  
  
-   Chiffrement activé : (True ou False)  
  
-   Mode de récupération : (simple, complet ou en utilisant les journaux de transactions)  
  
-   Dernière heure signalée : cette colonne affiche l'heure et la date locales du processeur à l'aide du type de données datetime. Pour plus d’informations, consultez la rubrique [datetime (Transact-SQL)](http://go.microsoft.com/fwlink/?LinkId=164071) . Lorsque vous utilisez le modèle d'objet de l'utilitaire, notez que SSMS utilise le type de données datetimeoffset. Pour plus d’informations, consultez la rubrique [datetimeoffset (Transact-SQL)](http://go.microsoft.com/fwlink/?LinkId=141713) .

## <a name="managed-instance-details-sql-server-utility"></a>Détails de l'instance gérée (utilitaire SQL Server)
 Les informations de la vue Instances managées de l’Explorateur de l’utilitaire fournissent des données d’utilisation pour les instances individuelles de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], un historique d’utilisation du processeur, les détails de l’utilisation du stockage au niveau du fichier, et la possibilité d’afficher et de mettre à jour des seuils de stratégie. Les seuils de stratégie peuvent être contrôlés au niveau de l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , pour un ordinateur, pour les fichiers de base de données et les fichiers journaux, ainsi qu’au niveau des volumes de stockage. Vous pouvez également consulter les détails des propriétés des instances managées individuelles de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
### <a name="uielement-list"></a>Liste des éléments de l'interface utilisateur  
 Mode Liste  
 Le mode Liste, dans le volet supérieur, affiche des données sur les instances individuelles de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] répertoriées dans les lignes par ComputerName\InstanceName.  
  
 Les icônes d'état d'intégrité fournissent un résumé de l'état de chaque instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] par catégorie d'utilisation :  
  
-   Coche verte (![](../../relational-databases/manage/media/well-utilized.gif "Correctement exploité")) : nombre d’instances gérées de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui ne violent pas les stratégies d’utilisation des ressources. Les ressources sont bien utilisées.  
  
-   Flèche bas verte (![](../../relational-databases/manage/media/utility-down-arrow.gif "Utilitaire, flèche bas")) : les ressources sont sous-exploitées.  
  
-   Flèche haut rouge (![](../../relational-databases/manage/media/utility-up-arrow.gif "Utilitaire, flèche haut")) : les ressources sont surexploitées.  
  
 La séquence des colonnes en mode Liste peut être modifiée en les faisant glisser vers la gauche ou la droite. Les colonnes en mode Liste peuvent être ajoutées ou supprimées en cliquant avec le bouton droit sur leurs en-tête et en les sélectionnant ou désélectionnant. Le menu contextuel offre également des options de tri. Le tri peut également être activé en cliquant en haut d'un nom de colonne.  
  
 Pour accéder aux options de filtre du mode Liste de l’utilitaire, cliquez avec le bouton droit sur le nœud **Instances managées** dans le volet de navigation de l’Explorateur de l’utilitaire et sélectionnez **Filtre**. Une fois que les paramètres de filtrage sont implémentés, le nœud **Instances managées** de l’Explorateur de l’utilitaire est étiqueté **Instances managées (filtré)**. Pour plus d’informations, consultez [Paramètres de filtre &#40;Explorateur d’objets et Explorateur de l’utilitaire&#41;](http://msdn.microsoft.com/library/4aab04bc-e1ab-4d4b-ab74-b287fc805bc2).  
  
 Par défaut, les colonnes suivantes affichent des informations sur l'état d'intégrité de chaque instance managée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   Processeur de l’instance : affiche l’état d’intégrité de l’utilisation du processeur alloué à cette instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. L'état d'intégrité de ce paramètre est déterminé d'après la stratégie d'utilisation du processeur définie pour l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et le paramètre de configuration de la stratégie d'évaluation des ressources volatiles. Pour plus d’informations, consultez [Réduire le bruit dans les stratégies d’utilisation du processeur &#40;utilitaire SQL Server&#41;](../../relational-databases/manage/reduce-noise-in-cpu-utilization-policies-sql-server-utility.md).  
  
     Pour consulter l’historique de l’utilisation du processeur pour cette instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ou afficher ou modifier les limites de la stratégie, cliquez sur l’onglet **Utilisation du processeur**.  
  
-   Processeur de l'ordinateur : affiche l'état d'intégrité de l'utilisation du processeur de l'ordinateur. L'état d'intégrité de ce paramètre est déterminé en fonction de la stratégie d'utilisation du processeur définie pour l'ordinateur et du paramètre de configuration de la stratégie d'évaluation des ressources volatiles. Pour plus d’informations, consultez [Réduire le bruit dans les stratégies d’utilisation du processeur &#40;utilitaire SQL Server&#41;](../../relational-databases/manage/reduce-noise-in-cpu-utilization-policies-sql-server-utility.md).  
  
     Pour consulter l’historique de l’utilisation du processeur pour cette instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ou afficher ou modifier les limites de la stratégie, cliquez sur l’onglet **Utilisation du processeur**.  
  
-   Espace de fichier : affiche un résumé de l’état d’intégrité de l’utilisation de l’espace de fichier pour toutes les bases de données appartenant à l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]sélectionnée. Si l'état d'intégrité de l'une des bases de données est surexploité, l'état d'intégrité de l'espace de fichier sera signalé en mode Liste comme étant surexploité. Si l'état d'intégrité de l'une des bases de données est sous-exploité et qu'aucune base de données n'est surexploitée, l'état d'intégrité de l'espace de fichier sera signalé en mode Liste comme étant sous-exploité. Sinon, l'état d'intégrité de l'espace du fichier sera signalé en mode Liste comme étant correctement utilisé. Pour afficher ou modifier les limites de la stratégie, cliquez sur l’onglet **Utilisation du stockage** .  
  
-   Espace de volume : affiche un résumé des états d'intégrité de l'utilisation du volume pour tous les volumes contenant des bases de données qui appartiennent à l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]sélectionnée. Si l'état d'intégrité d'un des volumes est surexploité, l'état d'intégrité de l'espace de volume sera signalé en mode Liste comme étant surexploité. Si l'état d'intégrité d'un des volumes de données est sous-exploité et qu'aucun volume n'est surexploité, l'état d'intégrité de l'espace de volume sera signalé en mode Liste comme étant sous-exploité. Sinon, l'état d'intégrité de l'espace de volume sera signalé en mode Liste comme étant correctement utilisé. Pour afficher ou modifier les limites de la stratégie, cliquez sur l’onglet **Utilisation du stockage** .  
  
-   Type de stratégie : indique si des stratégies « Globales » par défaut ou des stratégies de « Substitution » personnalisées sont appliquées pour l'instance managée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Autres colonnes pouvant s'afficher à l'aide du menu contextuel dans la zone d'en-tête de colonne du mode Liste :  
  
-   Nom du processeur :  
  
-   Vitesse du processeur (MHz) :  
  
-   Nombre de processeurs :  
  
-   Mémoire physique (Mo) :  
  
-   Version du système d'exploitation :  
  
-   Version de SQL Server :  
  
-   Édition de SQL Server :  
  
-   Cluster : (True ou False)  
  
-   Répertoire de sauvegarde :  
  
-   Classement :  
  
-   Respecter la casse : (True ou False)  
  
-   Langue :  
  
-   Dernière heure signalée : cette colonne affiche l'heure et la date locales du processeur à l'aide du type de données datetime. Pour plus d’informations, consultez la rubrique [datetime (Transact-SQL)](http://go.microsoft.com/fwlink/?LinkId=164071) . Lorsque vous utilisez le modèle d'objet de l'utilitaire, notez que SSMS utilise le type de données datetimeoffset. Pour plus d’informations, consultez la rubrique [datetimeoffset (Transact-SQL)](http://go.microsoft.com/fwlink/?LinkId=141713) .  
  
 Onglet Utilisation du processeur  
 L’onglet d’utilisation du processeur affiche côte à côte des graphiques de données d’historique pour l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et l’utilisation du processeur de l’ordinateur.  
  
 Le graphique affiche l'historique de l'utilisation du processeur pour l'intervalle spécifié dans les cases d'option situées sur le côté droit de la zone d'affichage. Pour modifier l'intervalle d'affichage et actualiser le jeu de données, sélectionnez une case d'option sur la liste.  
  
 Les options d'intervalle sont les suivantes :  
  
-   1 jour, affiché par intervalles de 15 minutes ;  
  
-   1 semaine, affiché par intervalles de 1 jour ;  
  
-   1 mois, affiché par intervalles de 1 semaine ;  
  
-   1 an, affiché par intervalles de 1 mois.  
  
 Onglet Utilisation du stockage  
 L'arborescence de l'onglet Utilisation du stockage affiche les détails de l'utilisation du stockage. Notez que les données temporelles affichent la date et l'heure locales du processeur à l'aide du type de données datetime. Pour plus d’informations, consultez la rubrique [datetime (Transact-SQL)](http://go.microsoft.com/fwlink/?LinkId=164071) . Lorsque vous utilisez le modèle d'objet de l'utilitaire, notez que SSMS utilise le type de données datetimeoffset. Pour plus d’informations, consultez la rubrique [datetimeoffset (Transact-SQL)](http://go.microsoft.com/fwlink/?LinkId=141713) .  
  
 L'affichage peut être regroupé par base de données ou par volume. Pour utiliser l’arborescence de la base de données, sélectionnez la case d’option **Base de données** dans la sélection **Regrouper les fichiers par** . Pour consulter l'état de l'utilisation du stockage pour des fichiers de base de données individuels, cliquez sur le signe plus en regard du nom d'une base de données dans l'arborescence. Les fichiers de base de données répertoriés incluent toutes les bases de données utilisateur et système qui appartiennent à l’instance managée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que vous avez sélectionnée en mode Liste.  
  
 L'arborescence correspond à la hiérarchie du stockage. Le nœud racine de l'arborescence est la base de données. Le niveau suivant de l'arborescence contient un nœud groupe de fichiers ou des nœuds qui appartiennent à la base de données, ainsi qu'un nœud fichier journal pour les journaux appartenant à la base de données. Le niveau suivant contient les fichiers de données qui appartiennent au groupe de fichiers.  
  
 Les données affichées dans le graphique d'historique de l'utilisation du stockage changent en fonction du nœud sélectionné dans l'arborescence :  
  
-   Sélectionnez le nœud groupe de fichiers pour afficher un graphique de l'espace de fichier utilisé par tous les fichiers de données appartenant au groupe de fichiers sélectionné.  
  
-   Sélectionnez le nœud fichier journal pour afficher un graphique de l'espace de fichier utilisé par tous les fichiers journaux appartenant à la base de données sélectionnée.  
  
-   Sélectionnez le nœud base de données pour afficher un graphique qui additionne l'espace de fichier utilisé par tous les fichiers de données et tous les fichiers journaux qui appartiennent à la base de données sélectionnée.  
  
 Les icônes d'état d'intégrité fournissent en un coup d'œil un état des fichiers, des groupes de fichiers et des fichiers journaux de la base de données :  
  
 Pour les bases de données :  
  
-   Coche verte : état d'intégrité de tous les groupes de fichiers et fichiers journaux ni surexploités ni sous-exploités.  
  
-   Flèche bas verte : état d'intégrité montrant qu'au moins un groupe de fichiers ou fichier journal est sous-exploité, aucun état d'intégrité n'étant surexploité.  
  
-   Flèche haut rouge : état d'intégrité montrant qu'au moins un groupe de fichiers ou fichier journal est surexploité.  
  
 Pour les groupes de fichiers ou les fichiers journaux :  
  
-   Coche verte : l'utilisation de l'espace de fichier de tous les fichiers du groupe de fichiers n'est ni surexploitée ni sous-exploitée.  
  
-   Flèche bas verte : l'utilisation de l'espace de fichier d'au moins un fichier du groupe de fichiers est sous-exploitée et aucun fichier du groupe de fichiers n'est surexploité.  
  
-   Flèche haut rouge : l'utilisation de l'espace de fichier de tous les fichiers de données du groupe de fichiers est surexploitée.  
  
 Pour consulter les fichiers par volume, sélectionnez la case d’option **Volume** dans la sélection **Regrouper les fichiers par** . Le graphique d'historique de l'utilisation du stockage montre l'espace de fichier utilisé par tous les fichiers de données et tous les fichiers journaux du volume de stockage. Développez l'arborescence pour voir le détail des fichiers des données et des fichiers journaux de la base de données.  
  
 Les icônes d'état servent à fournir l'état de chaque volume :  
  
-   Coche verte : les ressources sont bien utilisées.  
  
-   Flèche bas verte : les ressources sont sous-exploitées.  
  
-   Flèche haut rouge : les ressources sont surexploitées.  
  
 La jauge en regard de chaque nom de fichier sous l'onglet Utilisation du stockage représente la quantité d'espace utilisée par le fichier par rapport à sa capacité effective. La valeur en pourcentage affichée en regard de la jauge est le rapport entre la quantité d'espace utilisée par le fichier et la capacité effective du fichier. Les info-bulles fournissent des données qui servent à calculer le pourcentage de chaque volume et de chaque fichier par rapport à la capacité effective.  
  
 Si le fichier n'est pas configuré pour s'agrandir automatiquement, alors la capacité effective sera la quantité d'espace allouée au fichier. Si le fichier est configuré pour s'agrandir automatiquement, la capacité effective sera la somme de la quantité d'espace actuellement allouée au fichier et de la quantité d'espace libre actuellement disponible sur le volume.  
  
 Les stratégies d'utilisation du stockage peuvent être configurées pour les fichiers de données et pour les fichiers journaux. Pour afficher ou modifier les seuils de la stratégie d’utilisation du stockage pour les fichiers, cliquez sur le lien **Stratégie Fichiers** au bas du volet Utilisation du stockage. Pour afficher ou modifier les seuils de la stratégie d’utilisation du stockage pour un volume de stockage, cliquez sur le lien **Stratégie Volumes** au bas du volet Utilisation du stockage.  
  
 Pour remplacer les seuils de stratégie par défaut, cliquez sur la case d’option pour **Remplacer la stratégie par défaut**, spécifiez des valeurs pour les limites supérieures et inférieures, puis cliquez sur **OK**.  
  
 Pour plus d’informations sur la modification de la tolérance des violations de stratégie, consultez [Réduire le bruit dans les stratégies d’utilisation du processeur &#40;Utilitaire SQL Server&#41;](../../relational-databases/manage/reduce-noise-in-cpu-utilization-policies-sql-server-utility.md).  
  
 Onglet Détails des propriétés  
 Les détails de propriété répertoriés pour les instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] incluent les catégories suivantes :  
  
-   Nom du processeur :  
  
-   Vitesse du processeur (MHz) :  
  
-   Nombre de processeurs :  
  
-   Mémoire physique (Mo) :  
  
-   Version du système d'exploitation :  
  
-   Version de SQL Server :  
  
-   Édition de SQL Server :  
  
-   Cluster : (True ou False)  
  
-   Répertoire de sauvegarde :  
  
-   Classement :  
  
-   Respecter la casse : (True ou False)  
  
-   Langue :  

## <a name="utility-administration-sql-server-utility"></a>Administration de l'utilitaire (utilitaire SQL Server)
Utilisez les onglets Administration de l'utilitaire pour gérer les paramètres de stratégie, de sécurité et d'entrepôt de données pour un utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Pour plus d’informations sur les concepts de l’utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , consultez [Fonctionnalités et tâches de l’utilitaire SQL Server](../../relational-databases/manage/sql-server-utility-features-and-tasks.md).  
  
### <a name="uielement-list"></a>Liste des éléments de l'interface utilisateur
 **Onglet Stratégie** : utilisez l’onglet de stratégie pour afficher ou spécifier des stratégies d’analyse globales.  
  
 Définissez des stratégies globales de surveillance des applications de couche Données. Pour développer la liste des valeurs de cette option, cliquez sur la flèche en regard du nom de la stratégie ou cliquez sur le titre de la stratégie.  
 Quand une application se trouve-t-elle à court de capacité de processeur ? Pour modifier cette stratégie, utilisez le contrôle situé à droite de la description de la stratégie, puis cliquez sur **Appliquer**. Vous pouvez également restaurer les valeurs par défaut ou ignorer les modifications à l'aide des boutons situés en bas de l'affichage.  
  
-   La valeur maximale par défaut pour l'utilisation du processeur est de 70 %.  
  
-   La valeur minimale par défaut pour l'utilisation du processeur est de 0 %.  
  
 Quand une application se trouve-t-elle à court d'espace de fichier ? Pour modifier la stratégie d’utilisation de l’espace du fichier de données ou du fichier journal, utilisez le contrôle situé à droite de la description de la stratégie, puis cliquez sur **Appliquer**. Vous pouvez également restaurer les valeurs par défaut ou ignorer les modifications à l'aide des boutons situés en bas de l'affichage.  
  
-   La valeur maximale par défaut pour l'utilisation de l'espace du fichier est de 70 %.  
  
-   La valeur minimale par défaut pour l'utilisation de l'espace du fichier est de 0 %.  
  
 Définissez des stratégies globales de surveillance des applications d'instance gérée de SQL Server. Pour développer la liste des valeurs de cette option, cliquez sur la flèche en regard du nom de la stratégie ou cliquez sur le titre de la stratégie.  
 Quand une instance gérée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se trouve-t-elle à court de capacité de processeur ? Pour modifier cette stratégie, utilisez le contrôle situé à droite de la description de la stratégie, puis cliquez sur **Appliquer**. Vous pouvez également restaurer les valeurs par défaut ou ignorer les modifications à l'aide des boutons situés en bas de l'affichage.  
  
-   La valeur maximale par défaut pour l'utilisation d'une instance du processeur est de 70 %.  
  
-   La valeur minimale par défaut pour l'utilisation d'une instance du processeur est de 0 %.  
  
 Quand une instance gérée d'un ordinateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se trouve-t-elle à court de capacité de processeur ? Pour modifier cette stratégie, utilisez le contrôle situé à droite de la description de la stratégie, puis cliquez sur **Appliquer**. Vous pouvez également restaurer les valeurs par défaut ou ignorer les modifications à l'aide des boutons situés en bas de l'affichage.  
  
-   La valeur maximale par défaut pour l'utilisation du processeur de l'ordinateur est de 70 %.  
  
-   La valeur minimale par défaut pour l'utilisation du processeur de l'ordinateur est de 0 %.  
  
 Quand une instance gérée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est-elle à court d'espace de fichier ? Pour modifier la stratégie d’utilisation de l’espace du fichier de données ou du fichier journal, utilisez le contrôle situé à droite de la description de la stratégie, puis cliquez sur **Appliquer**. Vous pouvez également restaurer les valeurs par défaut ou ignorer les modifications à l'aide des boutons situés en bas de l'affichage.  
  
-   La valeur maximale par défaut pour l'utilisation de l'espace du fichier est de 70 %.  
  
-   La valeur minimale par défaut pour l'utilisation de l'espace du fichier est de 0 %.  
  
 Quand une instance gérée d'un ordinateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se trouve-t-elle à court d'espace de volume de stockage ? Pour modifier cette stratégie, utilisez le contrôle situé à droite de la description de la stratégie, puis cliquez sur **Appliquer**. Vous pouvez également restaurer les valeurs par défaut ou ignorer les modifications à l'aide des boutons situés en bas de l'affichage.  
  
-   La valeur maximale par défaut pour l'utilisation de l'espace du volume de l'ordinateur est de 70 %.  
  
-   La valeur minimale par défaut pour l'utilisation de l'espace du volume de l'ordinateur est de 0 %.  
  
 Réduction du bruit des violations de la stratégie par des ressources très volatiles. Pour développer les contrôles de cette fonctionnalité, cliquez sur la flèche basse située sur le côté droit de l'écran.  
 Pour plus d’informations, consultez [Réduire le bruit dans les stratégies d’utilisation du processeur &#40;utilitaire SQL Server&#41;](../../relational-databases/manage/reduce-noise-in-cpu-utilization-policies-sql-server-utility.md)  
  
 
**Onglet de sécurité** : affiche les noms de connexion disposant d’autorisations pour administrer ou lire l’UCP.  
  
 Sélectionnez les connexions de l'UCP qui seront ajoutées au rôle de lecteur d'utilitaire.  
 Le privilège de lecteur d'utilitaire permet au compte d'utilisateur de :  
  
-   Connectez-vous à l'utilitaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
-   Affichez tous les points d'observation dans l'Explorateur de l'utilitaire dans SSMS.  
  
-   Affichez les paramètres sur le nœud Administration de l'utilitaire dans l'Explorateur de l'utilitaire dans SSMS.  
  
 Les administrateurs de l'utilitaire peuvent inscrire et supprimer des instances de SQL Server dans un utilitaire SQL Server. Ils peuvent aussi modifier des stratégies sur les instances gérées et modifier des paramètres d'administration sur l'UCP.  
  
 Pour être administrateur de l'utilitaire, vous devez avoir des privilèges de sysadmin sur l'instance de SQL Server. Pour ajouter ou modifier des comptes d'utilisateurs pour l'UCP de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], utilisez l'Explorateur d'objets dans SSMS pour ajouter l'utilisateur aux connexions serveur de l'instance de l'UCP de SQL Server. Pour plus d’informations, consultez [sp_addlogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlogin-transact-sql.md).  
  
 
**Onglet Entrepôt de données** : affiche les détails de configuration de l’entrepôt de données de gestion de l’utilitaire.  
  
 Rétention des données  
 Spécifiez la période de rétention des données pour les informations d'utilisation recueillies pour les instances managées de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La période par défaut est d'un an. La valeur minimale est d'un mois. La plus longue valeur prise en charge est de 2 ans.  
  
 Informations de configuration de l'entrepôt de données de l'utilitaire  
 Les paramètres de configuration suivants ne sont pas configurables dans cette version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]:  
  
-   Nom de l’UMDW : Sysutility_mdw_\<GUID>_DATA.  
  
-   fréquence de téléchargement du jeu d'éléments de collecte : toutes les 15 minutes.  
  
 Le répertoire UMDW est configurable : \<Lecteur_système:\Program Files\Microsoft SQL Server\MSSQL10_50.<Nom_UCP>\MSSQL\Data\\, où \<Lecteur_système est normalement le lecteur C:\. Le fichier journal, UMDW_\<GUID>_LOG, se trouve dans le même répertoire.  
  
> **REMARQUE :** l’emplacement du fichier UMDW (sysutility_mdw) peut être modifié à l’aide des opérations de détachement et d’attachement ou d’ALTER DATABASE. Nous recommandons l'utilisation d'ALTER DATABASE. Pour plus d’informations, consultez [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md).  
  
 Revenir aux valeurs par défaut prédéfinies  
 Pour rétablir les valeurs par défaut des paramètres de cet onglet, cliquez sur le bouton **Paramètres par défaut** , puis sur **Appliquer**.  
 
  
## <a name="reference"></a>Référence  
 [Créer un point de contrôle de l’utilitaire SQL Server &#40;utilitaire SQL Server&#41;](../../relational-databases/manage/create-a-sql-server-utility-control-point-sql-server-utility.md)  
  
 [Se connecter à un utilitaire SQL Server](../../relational-databases/manage/connect-to-a-sql-server-utility.md)  
  
 [Inscrire une instance de SQL Server &#40;utilitaire SQL Server&#41;](../../relational-databases/manage/enroll-an-instance-of-sql-server-sql-server-utility.md)  
  
 [Configurer des stratégies de contrôle d’intégrité &#40;utilitaire SQL Server&#41;](../../relational-databases/manage/configure-health-policies-sql-server-utility.md)  
  
 [Surveiller des instances de SQL Server dans l'utilitaire SQL Server](../../relational-databases/manage/monitor-instances-of-sql-server-in-the-sql-server-utility.md)  
  
## <a name="see-also"></a> Voir aussi  
 [Fonctionnalités et tâches de l'utilitaire SQL Server](../../relational-databases/manage/sql-server-utility-features-and-tasks.md)   
 [Résolution des problèmes liés à l’utilitaire SQL Server](http://msdn.microsoft.com/library/f5f47c2a-38ea-40f8-9767-9bc138d14453)  
  
  
