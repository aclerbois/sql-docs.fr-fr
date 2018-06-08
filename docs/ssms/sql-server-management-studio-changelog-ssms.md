---
title: SQL Server Management Studio - Journal des modifications (SSMS) | Microsoft Docs
ms.custom: ''
ms.date: 05/09/2018
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssms
ms.reviewer: ''
ms.suite: sql
ms.technology: ssms
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 3dc76cc1-3b4c-4719-8296-f69ec1b476f9
caps.latest.revision: 72
author: stevestein
ms.author: sstein
manager: craigg
ms.openlocfilehash: e5190c4bbd8e0ade4f32831f5d696cc6f26296e5
ms.sourcegitcommit: 38f8824abb6760a9dc6953f10a6c91f97fa48432
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="sql-server-management-studio---changelog-ssms"></a>SQL Server Management Studio - Journal des modifications (SSMS)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
Cet article fournit des détails sur les mises à jour, les améliorations et les correctifs de bogues des versions actuelles et précédentes de SSMS. Téléchargez les [versions précédentes de SSMS ci-dessous](#previous-ssms-releases).


## <a name="ssms-177download-sql-server-management-studio-ssmsmd"></a>[SSMS 17.7](download-sql-server-management-studio-ssms.md)

Numéro de version : 17.7<br>
Numéro de build : 14.0.17254.0<br>
Date de sortie : 9 mai 2018

### <a name="whats-new"></a>Nouveautés

**SSMS général**

Moniteur de réplication :   
- Le moniteur de réplication prend désormais en charge l’inscription d’un écouteur pour les scénarios où la base de données du serveur de publication et/ou la base de données du serveur de distribution font partie du groupe de disponibilité. Vous pouvez maintenant analyser les environnements de réplication quand la base de données du serveur de publication et ou la base de données de distribution font partie de groupes de disponibilité AlwaysOn. 
 
Azure SQL Data Warehouse : 
- Ajout de la prise en charge de l’emplacement de ligne rejetée pour les tables externes dans Azure SQL Data Warehouse. 

**Integration Services (IS)**

- Ajout d’une fonctionnalité de planification pour les packages SSIS déployés dans Azure SQL Database. Contrairement à SQL Server local et SQL Database Managed Instance (préversion), qui proposent SQL Server Agent comme planificateur de travaux de première classe, SQL Database n’intègre pas de planificateur. Cette nouvelle fonctionnalité SSMS propose une interface utilisateur bien connue qui s’apparente à celle de SQL Server Agent pour la planification des packages déployés dans SQL Database. Si vous utilisez SQL Database pour héberger la base de données de catalogues SSIS, SSISDB, vous pouvez utiliser cette fonctionnalité SSMS pour générer les pipelines, les activités et les déclencheurs Data Factory nécessaires à la planification de packages SSIS. Vous pouvez ensuite modifier et étendre ces objets dans Data Factory. Pour plus d’informations, consultez [Planifier l’exécution d’un package SSIS sur Azure SQL Database à l’aide de SSMS](../integration-services/lift-shift/ssis-azure-schedule-packages-ssms.md). Pour en savoir plus sur les pipelines, les activités et les déclencheurs Azure Data Factory, consultez [Pipelines et activités dans Azure Data Factory](https://docs.microsoft.com/azure/data-factory/concepts-pipelines-activities) et [Exécution de pipelines et déclencheurs dans Azure Data Factory](https://docs.microsoft.com/azure/data-factory/concepts-pipeline-execution-triggers).
- Prise en charge de la planification de packages SSIS dans SQL Agent sur une instance managée SQL. Il est désormais possible de créer des travaux SQL Agent pour exécuter des packages SSIS sur l’instance managée. 

### <a name="bug-fixes"></a>Correctifs de bogues

**SSMS général** 

Plan de maintenance :   
- Correction d’un problème où toute tentative de modification de la planification d’un plan de maintenance existant se traduisait par la levée d’une exception. Pour plus d’informations, consultez [SSMS 17.6 crashes when clicking on a schedule in a maintenance plan](https://feedback.azure.com/forums/908035-sql-server/suggestions/33712924).

AlwaysOn : 
- Correction d’un problème où le tableau de bord de latence AlwaysOn ne fonctionnait pas avec SQL Server 2012.
 
Création de scripts : 
- Correction d’un problème où la création d’un script de procédure stockée pour Azure SQL Data Warehouse ne fonctionnait pas pour un utilisateur non administrateur.
- Correction d’un problème où la création d’un script de base de données pour Azure SQL Database ne donnait pas lieu à la création d’un script pour les propriétés *SCOPED CONFIGURATION*.
 
Télémétrie : 
- Correction d’un problème où SSMS se bloquait au moment de se connecter à un serveur après avoir renoncé à l’envoi de données de télémétrie.
 
Azure SQL Database : 
- Correction d’un problème où l’utilisateur ne pouvait pas définir ou modifier le niveau de compatibilité (liste déroulante vide). Remarque : pour définir le niveau de compatibilité à 150, l’utilisateur doit toujours utiliser le bouton *Script* et modifier manuellement le script. 
 
SMO : 
- Paramètre Taille du journal des erreurs affiché dans SMO. Pour plus d’informations, consultez [Set the Maximum Size of the SQL Server Error Logs](https://feedback.azure.com/forums/908035-sql-server/suggestions/33624115).  
- Correction de la création de scripts de sauts de ligne dans SMO sur Linux.
- Diverses améliorations sur le plan des performances pendant la récupération de propriétés rarement utilisées.  

IntelliSense : 
- Amélioration des performances : réduction du volume de requêtes IntelliSense pour les données de colonne. Cela est particulièrement utile quand les tables utilisées contiennent une multitude de colonnes. 

Paramètres utilisateur SSMS :
- Correction d’un problème où la page d’options ne se redimensionnait pas correctement.

Divers :  
- Amélioration de l’affichage de texte dans la page de *détails des statistiques*. 

**Integration Services (IS)**

- Meilleure prise en charge d’Azure SQL Database Managed Instance.
- Correction d’un problème où l’utilisateur ne pouvait pas créer de catalogue pour SQL Server 2014 ou version antérieure.
- Correction de deux problèmes liés aux rapports :
   - Suppression du nom d’ordinateur pour les serveurs Azure.
   - Gestion améliorée du nom d’objet localisé.


### <a name="known-issues"></a>Problèmes connus

Certaines boîtes de dialogue affichent une erreur d’édition non valide quand les nouvelles éditions *Usage général* ou *Critique pour l’entreprise* d’Azure SQL Database sont utilisées.

## <a name="downloadssdtmediadownloadpng-ssms-176httpsgomicrosoftcomfwlinklinkid870039"></a>![télécharger](../ssdt/media/download.png) [SSMS 17.6](https://go.microsoft.com/fwlink/?linkid=870039)

Numéro de build : 14.0.17230.0<br>
Date de sortie : 20 mars 2018

[Chinois (République populaire de Chine)](https://go.microsoft.com/fwlink/?linkid=870039&clcid=0x804) | [Chinois (Taïwan)](https://go.microsoft.com/fwlink/?linkid=870039&clcid=0x404) | [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=870039&clcid=0x409) | [Français](https://go.microsoft.com/fwlink/?linkid=870039&clcid=0x40c) | [Allemand | ](https://go.microsoft.com/fwlink/?linkid=870039&clcid=0x407)[Italien](https://go.microsoft.com/fwlink/?linkid=870039&clcid=0x410) | [Japonais](https://go.microsoft.com/fwlink/?linkid=870039&clcid=0x411) | [Coréen](https://go.microsoft.com/fwlink/?linkid=870039&clcid=0x412) | [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=870039&clcid=0x416) | [Russe](https://go.microsoft.com/fwlink/?linkid=870039&clcid=0x419) | [Espagnol](https://go.microsoft.com/fwlink/?linkid=870039&clcid=0x40a)

### <a name="whats-new"></a>Nouveautés

**SSMS général**

SQL Database Managed Instance :

- Ajout de la prise en charge d’[Azure SQL Database Managed Instance](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance). Azure SQL Database Managed Instance (en préversion) est une nouvelle version d’Azure SQL Database offrant une compatibilité proche de 100 % avec SQL Server local, une implémentation native de [réseau virtuel (VNet)](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) qui traite les problèmes de sécurité courants et un [modèle d’entreprise](https://azure.microsoft.com/pricing/details/sql-database/) propice pour les clients de SQL Server local.
- Prise en charge de scénarios de gestion courants tels que les suivants :
   - Créer et modifier des bases de données.
   - Sauvegarder et restaurer des bases de données.
   - Importation, exportation, extraction et publication d’applications de la couche Données.
   - Affichage et modification de propriétés du serveur.
   - Prise en charge complète de l’Explorateur d’objets.
   - Écriture d’objets de base de données sous forme de scripts.
   - Prise en charge des travaux de l’Agent SQL.
   - Prise en charge des serveurs liés.
- Vous pouvez en savoir plus sur les instances managées [ici](https://azure.microsoft.com/blog/migrate-your-databases-to-a-fully-managed-service-with-azure-sql-database-managed-instance/).

Explorateur d'objets :
- Paramètres ajoutés pour ne pas imposer des crochets autour des noms lors d’un glisser-déplacer à partir de l’Explorateur d’objets vers une fenêtre de requête. (Suggestions des utilisateurs [32911933](https://feedback.azure.com/forums/908035-sql-server/suggestions/32911933) et [32671051](https://feedback.azure.com/forums/908035-sql-server/suggestions/32671051).)

Classification des données :
- Améliorations générales et résolutions de bogues.

**Integration Services (IS)**

- Ajout de la prise en charge du déploiement de packages sur une instance [SQL Database Managed Instance](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance).

### <a name="bug-fixes"></a>Correctifs de bogues

**SSMS général**

Classification des données :

- Correction d’un problème dans *Classification des données* à cause duquel les classifications nouvellement ajoutées s’affichaient avec un *type d’informations* et une *étiquette sensibilité* obsolètes.
- Résolution d’un problème où la *Classification des données* ne fonctionnait pas quand la cible était un serveur configuré avec un classement qui respecte la casse.
        
AlwaysOn :

- Correction d’un problème dans le tableau de bord d’affichage des groupes de disponibilité où le fait de cliquer sur *Collecter les données de latence* pouvait générer une erreur quand le serveur était configuré avec un classement qui respecte la casse.
- Correction d’un problème où SSMS signalait de façon erronée un groupe de disponibilité comme *Distribué* quand le service de cluster s’arrêtait.
- Correction d’un problème survenant lors de la création d’un groupe de disponibilité à l’aide de la boîte de dialogue *Créer le groupe de disponibilité* et où la valeur *ReadOnlyRoutingUrl* est exigée.
- Correction d’un problème survenant quand le serveur principal est en panne et que le basculement manuel vers un serveur secondaire lève une exception NullReferenceException.
- Correction d’un problème survenant lors de la création d’un groupe de disponibilité à l’aide de la fonctionnalité de sauvegarde/restauration pour initialiser une base de données et que les fichiers de base de données sont créés dans le répertoire par défaut sur les réplicas secondaires. Le correctif inclut les opérations suivantes :
   - Ajouter le validateur de répertoire de données/journaux.
   - Effectuer le déplacement des fichiers vers le réplica principal uniquement quand le réplica se trouve sur un autre système d’exploitation.
- Résolution d’un problème où l’Assistant SSMS ne génère pas l’option *CLUSTER_TYPE*, ce qui provoquait l’échec de la jointure secondaire.

Programme d’installation :
- Correction d’un problème où une tentative de mise à niveau de SSMS en installant le « package de mise à niveau » échouait quand SSMS était installé dans un emplacement autre que celui par défaut.

SMO :
- Correction d’un problème de performances où l’écriture du script des tables sur SQL Server 2016 et les versions ultérieures peut prendre jusqu’à 30 secondes (elle prend actuellement moins d’une seconde).

Explorateur d'objets :
- Correction d’un problème où SSMS pouvait lever une exception indiquant, par exemple, que « l’objet ne peut pas être converti de du type DBNull en d’autres types » quand vous tentez de développer le nœud *Gestion* dans l’Explorateur d’objets.
- Correction d’un problème où *Démarrer PowerShell* ne détectait pas le module SQLServer quand un profil PS défini par l’utilisateur émettait la sortie.
- Correction d’un blocage intermittent qui pouvait se produire lors d’un clic droit sur un nœud Table ou Index dans l’Explorateur d’objets.

Messagerie de base de données :
- Correction d’un problème où l’*Assistant Configuration de la messagerie de base de données* levait une exception en cas de tentative d’affichage/de gestion de plus de 16 profils.


**Analysis Services (AS)**

- Correction d’un problème où en cas de modification d’une source de données sur un modèle de niveau de compatibilité 1400 dans SSMS, les changements apportés n’étaient pas enregistrés sur le serveur.

**Integration Services (IS)**

- Correction d’un problème où SSMS n’affichait pas de nœud de catalogue SSIS ni de rapports quand une connexion était établie à SQL Database Managed Instance

### <a name="known-issues"></a>Problèmes connus

> [!WARNING]
> Il existe un problème connu où SSMS 17.6 devient instable et plante lors de l’utilisation de [Plans de maintenance](../relational-databases/maintenance-plans/maintenance-plans.md). Si vous utilisez des plans de maintenance, n’installez pas SSMS 17.6. Revenez à SSMS 17.5 si vous avez déjà installé 17.6 et que ce problème vous affecte. 

## <a name="previous-ssms-releases"></a>Versions précédentes de SSMS

Téléchargez les versions précédentes de SSMS en cliquant sur le lien des titres dans les sections suivantes.

## <a name="downloadssdtmediadownloadpng-ssms-175httpsgomicrosoftcomfwlinklinkid867670"></a>![télécharger](../ssdt/media/download.png) [SSMS 17.5](https://go.microsoft.com/fwlink/?linkid=867670)
Disponibilité générale | Numéro de build : 14.0.17224.0

[Chinois (République populaire de Chine)](https://go.microsoft.com/fwlink/?linkid=867670&clcid=0x804) | [Chinois (Taïwan)](https://go.microsoft.com/fwlink/?linkid=867670&clcid=0x404) | [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=867670&clcid=0x409) | [Français](https://go.microsoft.com/fwlink/?linkid=867670&clcid=0x40c) | [Allemand | ](https://go.microsoft.com/fwlink/?linkid=867670&clcid=0x407)[Italien](https://go.microsoft.com/fwlink/?linkid=867670&clcid=0x410) | [Japonais](https://go.microsoft.com/fwlink/?linkid=867670&clcid=0x411) | [Coréen](https://go.microsoft.com/fwlink/?linkid=867670&clcid=0x412) | [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=867670&clcid=0x416) | [Russe](https://go.microsoft.com/fwlink/?linkid=867670&clcid=0x419) | [Espagnol](https://go.microsoft.com/fwlink/?linkid=867670&clcid=0x40a)

### <a name="whats-new"></a>Nouveautés

**SSMS général**

Découverte et classification des données :

- Ajout d’une nouvelle fonctionnalité Découverte et classification des données SQL pour la découverte, la classification, l’étiquetage et la création de rapport concernant les données sensibles dans vos bases de données. 
- La découverte et la classification automatiques de vos données les plus sensibles (professionnelles, financières, médicales, PII, etc.) peuvent jouer un rôle essentiel dans la protection des informations de votre organisation.
- Découvrez plus d’informations sur la [Découverte et classification des données SQL](../relational-databases/security/sql-data-discovery-and-classification.md).

Éditeur de requête :

- Ajout de la prise en charge de l’option SkipRows pour le format de fichier externe de texte délimité dans Azure SQL Data Warehouse. Cette fonctionnalité permet aux utilisateurs d’ignorer un nombre spécifié de lignes pendant le chargement de fichiers de texte délimité dans SQL DW. Ajout également de la prise en charge d’Intellisense/SMO correspondante pour le mot clé FIRST_ROW. 

Plan d’exécution de requêtes :

- Activation de l’affichage du bouton de plan estimé pour SQL Data Warehouse
- Ajout d’un nouvel attribut de plan d’exécution de requêtes *EstimateRowsWithoutRowGoal* et ajout de nouveaux attributs de plan d’exécution de requêtes pour *QueryTimeStats* : *UdfCpuTime* et *UdfElapsedTime*. Pour plus d’informations, consultez [Informations sur l’objectif de lignes de l’optimiseur dans le plan d’exécution de requêtes ajouté dans SQL Server 2017 CU3](http://support.microsoft.com/help/4051361).



### <a name="bug-fixes"></a>Correctifs de bogues

**SSMS général**

Plan d’exécution de requêtes :

- Correction du temps écoulé des statistiques des requêtes actives, pour afficher la durée d’exécution du moteur plutôt que le temps écoulé de la connexion LQS.
- Correction d’un problème où le plan d’exécution de requêtes ne pouvait pas reconnaître les opérateurs logiques Apply comme GbApply et InnerApply.
- Correction d’un problème lié à ExchangeSpill.

Éditeur de requête :

- Correction d’un problème lié aux SPID où SSMS pouvait générer une erreur de type « Le format de la chaîne d’entrée est incorrect. (mscorlib) » à l’exécution d’une requête simple précédée de « SET SHOWPLAN_ALL ON ». 


SMO :

- Correction d’un problème où SMO ne pouvait pas récupérer les propriétés AvailabilityReplica quand le classement du serveur était sensible à la casse (par conséquent, SSMS pouvait afficher un message d’erreur de type « L’identificateur en plusieurs parties « a.delimited » ne peut pas être lié ».
- Correction d’un problème dans la classe DatabaseScopedConfigurationCollection, où les classements n’étaient pas gérés correctement (par conséquent, un SSMS s’exécutant sur un ordinateur avec des paramètres régionaux turcs pouvait afficher une erreur de type « L’estimation de cardinalité héritée n’est pas une configuration délimitée valide » lors d’un clic droit sur une base de données s’exécutant sur un serveur avec un classement sensible à la casse).
- Correction d’un problème dans la classe JobServer, où SMO ne pouvait pas récupérer les propriétés de SQL Agent sur un serveur SQL 2005 (par conséquent, SSMS générait une erreur de type « Impossible d’attribuer une valeur par défaut à une variable locale. La variable scalaire « @ServiceStartMode » doit être déclarée » et, finalement, n’affichait pas le nœud SQL Agent dans l’Explorateur d’objets).

Modèles : 

- « Messagerie de base de données » : correction de quelques fautes de frappe [(https://feedback.azure.com/forums/908035/suggestions/33143512)](https://feedback.azure.com/forums/908035/suggestions/33143512).  

Explorateur d'objets :
- Correction d’un problème où la compression gérée échouait pour les index (https://feedback.azure.com/forums/908035-sql-server/suggestions/32610058-ssms-17-4-error-when-enabling-page-compression-o).

Audit :
- Correction d’un problème avec la fonctionnalité *Fusionner les fichiers d’audit*.
<br>

### <a name="known-issues"></a>Problèmes connus

Classification des données :
- La suppression d’une classification, puis l’ajout manuel d’une nouvelle classification pour la même colonne entraînait l’attribution de l’ancien type d’informations et de l’ancienne étiquette de sensibilité à la colonne dans la vue principale.<br>
*Solution de contournement* : Attribuez le nouveau type d’informations et la nouvelle étiquette de sensibilité après l’ajout de la classification à la vue principale et avant l’enregistrement.


## <a name="downloadssdtmediadownloadpng-ssms-174httpsgomicrosoftcomfwlinklinkid864329"></a>![télécharger](../ssdt/media/download.png) [SSMS 17.4](https://go.microsoft.com/fwlink/?linkid=864329)
Disponibilité générale | Numéro de build : 14.0.17213.0

[Chinois (République populaire de Chine)](https://go.microsoft.com/fwlink/?linkid=864329&clcid=0x804) | [Chinois (Taïwan)](https://go.microsoft.com/fwlink/?linkid=864329&clcid=0x404) | [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=864329&clcid=0x409) | [Français](https://go.microsoft.com/fwlink/?linkid=864329&clcid=0x40c) | [Allemand | ](https://go.microsoft.com/fwlink/?linkid=864329&clcid=0x407)[Italien](https://go.microsoft.com/fwlink/?linkid=864329&clcid=0x410) | [Japonais](https://go.microsoft.com/fwlink/?linkid=864329&clcid=0x411) | [Coréen](https://go.microsoft.com/fwlink/?linkid=864329&clcid=0x412) | [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=864329&clcid=0x416) | [Russe](https://go.microsoft.com/fwlink/?linkid=864329&clcid=0x419) | [Espagnol](https://go.microsoft.com/fwlink/?linkid=864329&clcid=0x40a)


### <a name="whats-new"></a>Nouveautés

**SSMS général**

Évaluation des vulnérabilités :
- Ajout d’un nouveau service d’évaluation des vulnérabilités SQL pour analyser la présence éventuelle de vulnérabilités et d’écarts par rapport aux meilleures pratiques dans les bases de données, par exemple, des erreurs de configuration, des autorisations excessives et des données sensibles exposées. 
- Les résultats de l’évaluation sont accompagnés d’étapes actionnables permettant de résoudre chaque problème et, le cas échéant, de scripts de correction personnalisés. Le rapport d’évaluation est adaptable à chaque environnement et aux exigences spécifiques. Pour en savoir plus, consultez la page [Évaluation des vulnérabilités SQL](https://docs.microsoft.com/sql/relational-databases/security/sql-vulnerability-assessment).

SMO :
- Correction du problème à cause duquel *HasMemoryOptimizedObjects* levait une exception sur Azure.
- Ajout de la prise en charge de la nouvelle fonctionnalité CATALOG_COLLATION.

Tableau de bord Always On :
- Amélioration de l’analyse de la latence dans les groupes de disponibilité.
- Ajout de deux nouveaux rapports : *AlwaysOn\_Latency\_Primary* et *AlwaysOn\_Latency\_Secondary*.

Plan d’exécution de requêtes :
- Mise à jour des liens, de sorte qu’ils pointent vers la documentation appropriée.
- Permet une analyse de plan unique directement à partir du plan réellement généré.
- Nouvel ensemble d’icônes.
- Ajout de la prise en charge permettant de reconnaître « Appliquer des opérateurs logiques », comme GbApply ou InnerApply.
        
XE Profiler :
- Renommé XEvent Profiler.
- À présent, les commandes de menu Arrêter/Démarrer arrêtent/démarrent la session par défaut.
- Activation des raccourcis clavier (par exemple, CTRL+F pour faire une recherche).
- Ajout des actions database\_name et client\_hostname aux événements concernés dans les sessions XEvent Profiler. Pour que la modification prenne effet, vous devrez peut-être supprimer les instances de sessions QuickSessionStandard et QuickSessionTSQL sur les serveurs - [Connect 3142981](https://connect.microsoft.com/SQLServer/feedback/details/3142981).

Ligne de commande :
- Ajout d’une nouvelle option de ligne de commande («-G ») qui permet de faire en sorte que SSMS se connecte automatiquement à un serveur/une base de données avec l’authentification Active Directory (« Intégré » ou « Mot de passe »). Pour plus d’informations, consultez la page [Utilitaire SSMS](ssms-utility.md).

Assistant Importation de fichier plat :
- Ajout de la possibilité de choisir un nom de schéma autre que le nom par défaut (« dbo ») lors de la création de la table.

Magasin des requêtes :
- Restauration du rapport « Requêtes ayant régressé » lors du développement de la liste de rapports disponibles dans le Magasin des requêtes.

**Integration Services (IS)**
- Ajout de la fonction de validation de package à l’Assistant Déploiement, ce qui permet à l’utilisateur d’identifier des composants à l’intérieur des packages SSIS non pris en charge dans Azure-SSIS IR.

### <a name="bug-fixes"></a>Correctifs de bogues

**SSMS général**

- Explorateur d'objets :
    - Correction du problème à cause duquel le nœud Fonction table ne s’affichait pas pour les captures instantanées de base de données - [Connect 3140161](https://connect.microsoft.com/SQLServer/feedback/details/3140161).
    - Amélioration des performances lors du développement du nœud *Bases de données* quand le serveur possède des bases de données autoclose.
- Éditeur de requête :
    - Correction du problème à cause duquel IntelliSense échouait pour les utilisateurs n’ayant pas accès à la base de données MASTER.
    - Correction d’un problème qui était à l’origine d’un blocage de SSMS dans certains cas, lors de la fermeture de la connexion à un ordinateur distant - [Connect 3142557](https://connect.microsoft.com/SQLServer/feedback/details/3142557).
- Observateur XEvent :
    - Réactivation de la fonctionnalité d’exportation vers XEL.
    - Correction des problèmes qui empêchaient l’utilisateur, dans certains cas de charger la totalité d’un fichier XEL.
- XEvent Profiler :
    - Correction d’un problème qui était à l’origine d’un blocage de SSMS lorsque l’utilisateur ne disposait pas des autorisations *VIEW SERVER STATE*.
    - Correction du problème à cause duquel la fermeture de la fenêtre Données actives XE Profiler n’arrêtait pas la session sous-jacente.
- Serveurs inscrits :
    - Correction du problème à cause duquel la commande « Déplacer vers... » cessait de fonctionner - [Connect 3142862](https://connect.microsoft.com/SQLServer/feedback/details/3142862) et [Connect 3144359](https://connect.microsoft.com/SQLServer/feedback/details/3144359/).
- SMO :
    - Correction du problème à cause duquel la méthode TransferData sur l’objet Transfer ne fonctionnait pas.
    - Correction du problème à cause duquel les bases de données de serveur levaient une exception pour les bases de données SQL Data Warehouse en pause.
    - Correction du problème à cause duquel le lancement de scripts de la base de données SQL sur SQL Data Warehouse générait des valeurs de paramètres T-SQL incorrectes.
    - Correction du problème à cause duquel les scripts d’une extension de base de données émettaient à tort l’option *DATA\_COMPRESSION*.
- Moniteur d’activité des travaux :
    - Correction du problème à cause duquel l’utilisateur obtenait une erreur « Index hors plage. Il doit être non négatif et inférieur à la taille de la collection. 
        Nom du paramètre : index (System.Windows.Forms) » lorsqu’il tentait de filtrer par catégorie - [Connect 3138691](https://connect.microsoft.com/SQLServer/feedback/details/3138691).
- Dialogue de connexion :
    - Correction du problème qui empêchait les utilisateurs de domaine ne disposant pas d’un accès en lecture/écriture à un contrôleur de domaine de se connecter à un serveur SQL Server avec l’authentification SQL - [Connect 2373381](https://connect.microsoft.com/SQLServer/feedback/details/2373381).
- Réplication :
    - Correction du problème à cause duquel une erreur du type « Impossible d’appliquer la valeur "Null" à la propriété ServerInstance » s’affichait au moment de consulter les propriétés d’un abonnement par extraction dans SQL Server.
- Configuration de SSMS :
    - Correction du problème à cause duquel la configuration de SSMS provoquait à tort la reconfiguration de tous les produits installés sur l’ordinateur.
- Paramètres utilisateurs :
   - Grâce à ce correctif, les utilisateurs du cloud souverain du gouvernement des États-Unis profiteront d’un accès ininterrompu à leurs ressources Azure SQL Database et ARM avec SSMS via l’authentification universelle et la connexion Azure Active Directory.  Les utilisateurs des versions antérieures de SSMS devront ouvrir Outils|Options|Services Azure et, sous Gestion des ressources, changer la configuration de la propriété « Autorité Active Directory » pour lui donner la valeur https://login.microsoftonline.us.

**Analysis Services (AS)**

- Profiler : correction d’un problème de connexion avec l’authentification Windows à Azure Analysis Services.
- Correction d’un problème qui pouvait provoquer un blocage en cas d’annulation des détails de connexion sur un modèle 1400.
- Lors de sa définition dans la boîte de dialogue Propriétés de connexion, dans le cadre de l’actualisation des informations d’identification, la clé Blob Azure sera désormais visuellement masquée.
- Correction d’un problème dans la boîte de dialogue de sélection de l’utilisateur Azure Analysis Services, pour afficher le GUID de l’ID application au lieu de l’ID objet lors de la recherche.
- Correction d’un problème dans la barre d’outils du concepteur de requêtes Parcourir la base de données/MDX, à cause duquel les icônes de certains boutons étaient incorrectement mappées.
- Correction d’un problème qui empêchait de se connecter à SSAS avec des adresses HTTP/HTTPS msmdpump IIS.
- Plusieurs chaînes de la boîte de dialogue Sélecteur d’utilisateur Azure Analysis Services ont maintenant été traduites dans des langues supplémentaires.
- La propriété MaxConnections est à présent visible pour les sources de données dans les modèles tabulaires.
- L’Assistant Déploiement générera désormais des définitions JSON correctes pour les membres du rôle Azure Analysis Services.
- Correction d’un problème dans SQL Profiler à cause duquel sélectionner l’authentification Windows à Azure Analysis Services invitait quand même à se connecter.



## <a name="downloadssdtmediadownloadpng-ssms-173httpsgomicrosoftcomfwlinklinkid858904"></a>![télécharger](../ssdt/media/download.png) [SSMS 17.3](https://go.microsoft.com/fwlink/?linkid=858904)
Disponibilité générale | Numéro de build : 14.0.17199.0

[Chinois (République populaire de Chine)](https://go.microsoft.com/fwlink/?linkid=858904&clcid=0x804) | [Chinois (Taïwan)](https://go.microsoft.com/fwlink/?linkid=858904&clcid=0x404) | [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=858904&clcid=0x409) | [Français](https://go.microsoft.com/fwlink/?linkid=858904&clcid=0x40c) | [Allemand | ](https://go.microsoft.com/fwlink/?linkid=858904&clcid=0x407)[Italien](https://go.microsoft.com/fwlink/?linkid=858904&clcid=0x410) | [Japonais](https://go.microsoft.com/fwlink/?linkid=858904&clcid=0x411) | [Coréen](https://go.microsoft.com/fwlink/?linkid=858904&clcid=0x412) | [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=858904&clcid=0x416) | [Russe](https://go.microsoft.com/fwlink/?linkid=858904&clcid=0x419) | [Espagnol](https://go.microsoft.com/fwlink/?linkid=858904&clcid=0x40a)


### <a name="enhancements"></a>Améliorations

- Ajout du nouvel Assistant « Importer un fichier plat » pour simplifier l’importation des fichiers CSV avec une infrastructure intelligente, nécessitant une intervention de l’utilisateur ou des connaissances techniques minimes. Pour plus d’informations, consultez [Assistant Importer un fichier plat dans SQL](../relational-databases/import-export/import-flat-file-wizard.md).
- Ajout du nœud « XEvent Profiler » à l’Explorateur d’objets. Pour plus d’informations, consultez [Utiliser SSMS XEvent Profiler](../relational-databases/extended-events/use-the-ssms-xe-profiler.md).
- Mise à jour du filtrage et de la catégorisation des attentes dans le rapport des attentes historiques du Tableau de bord Performances.
- Ajout de la vérification syntaxique de la fonction « Predict ».
- Ajout de la vérification syntaxique des requêtes External Library Management.
- Ajout de la prise en charge SMO pour External Library Management.
- Ajout de la prise en charge de « Démarrer PowerShell » dans la fenêtre « Serveurs inscrits » (nécessite un nouveau module SQL PowerShell).
- AlwaysOn : ajout de la [prise en charge du routage en lecture seule](../database-engine/availability-groups/windows/configure-read-only-routing-for-an-availability-group-sql-server.md) pour les groupes de disponibilité.
- Ajout d’une option pour envoyer les informations de trace dans la fenêtre Sortie pour les connexions « Active Directory - Authentification universelle avec prise en charge de MFA » (option désactivée par défaut ; doit être activée dans les paramètres utilisateur sous « Outils > Options > Services Azure > Azure Cloud > Niveau de trace dans la fenêtre Sortie ADAL »). 
- Magasin des requêtes : 
  - L’interface utilisateur Magasin des requêtes est maintenant accessible même quand QDS est désactivé, si QDS a enregistré des données.
  - L’interface utilisateur Magasin des requêtes expose maintenant les attentes par catégorie dans tous les rapports existants. Cela permet aux clients de gérer les scénarios avec des requêtes principales en attente, par exemple.
- Ajout des en-têtes de paramètres de script rendu facultatif (désactivé par défaut ; peut être activé dans les paramètres utilisateur sous « Outils > Options > Explorateur d’objets SQL Server > Scripts > Inclure l’en-tête des paramètres de script ») - [Article Connect 3139199](https://connect.microsoft.com/SQLServer/feedback/details/3139199).
- Suppression de la personnalisation « RC ».

### <a name="bug-fixes"></a>Correctifs de bogues

**SSMS général**

- XEvent : 
   - Correction d’un problème où SSMS ouvrait uniquement une partie des événements dans le fichier .xel.
   - Amélioration de l’expérience « Observer les données actives » quand la base de données par défaut n’est pas définie à 'master' - [Article Connect 1222582](https://connect.microsoft.com/SQLServer/feedback/details/1222582).
- AlwaysOn : correction du problème pouvant provoquer l’échec de la restauration des sauvegardes de journaux avec l’erreur « Le journal dans ce jeu de sauvegarde se termine au numéro de séquence d’enregistrement x, ce qui est trop tôt pour une application à la base de données ».
- Moniteur d’activité des travaux : correction des icônes incohérentes - [Article Connect 3133100](https://connect.microsoft.com/SQLServer/feedback/details/3133100).
- Magasin des requêtes : correction du problème qui empêchait l’utilisateur de choisir une plage de dates « personnalisée » pour les rapports du magasin des requêtes. Consultez les articles Connect suivants :
   - [Article Connect 3139842](https://connect.microsoft.com/SQLServer/feedback/details/3139842)
   - [Article Connect 3139399](http://connect.microsoft.com/SQLServer/feedback/details/3139399)
- Correction du problème où la boîte de dialogue de connexion n’« effaçait » pas la dernière base de données utilisée quand les informations enregistrées contenaient une base de données nommée et que l’utilisateur sélectionnait <default>.
- Scripts d’objets :
    - Correction d’un problème qui empêchait la commande « Générer le script de la base de données » de fonctionner et générait une erreur quand l’utilisateur avait une base de données DW suspendue sur le serveur, mais qu’il sélectionnait une autre base de données non DW et tentait de créer un script de cette dernière.
    - Correction du problème de non-correspondance entre l’en-tête des procédures stockées faisant l’objet d’un script et les paramètres du script, ce qui donnait un script équivoque - [Article Connect 3139784](http://connect.microsoft.com/SQLServer/feedback/details/3139784).
    - Réactivation du bouton « Script » lors du ciblage d’objets SQL Azure.
    - Correction du problème où SSMS ne permettait pas de créer un script pour les opérations « Modifier » ou « Exécuter » sur certains objets (UDF, View, SP, Trigger) avec une connexion à une base de données SQL Azure - [Article Connect 3136386](https://connect.microsoft.com/SQLServer/feedback/details/3136386).
- Éditeur de requête :
  - Amélioration d’Intellisense lors du ciblage de bases de données SQL Azure.
  - Correction d’un problème qui faisait échouer les requêtes en raison de l’expiration d’un jeton d’authentification (authentification universelle).
  - Amélioration d’Intellisense avec les bases de données Azure SQL. En particulier, dans le cas d’une connexion à Azure SQL Database, la dernière grammaire de T-SQL (140) est maintenant utilisée.
  - Correction d’un problème où l’ouverture d’une fenêtre de requête avec une connexion à une base de données autre que DataWarehouse sur un serveur générait différentes erreurs liées aux types/options non pris en charge pour toutes les fenêtres de requête ouvertes postérieurement sur ce serveur.
- AlwaysOn :
   - Ajout d’une colonne en mode amorçage au tableau de bord AlwaysOn et à la page de propriétés des groupes de disponibilité.
   - Correction d’un problème qui empêchait la création d’un groupe de disponibilité Linux quand le groupe de disponibilité principal était sur Windows - [Article Connect 3139856](https://connect.microsoft.com/SQLServer/feedback/details/3139856).
- Correction de plusieurs problèmes de « mémoire insuffisante » dans SSMS lors de l’exécution de requêtes - [Article Connect 2845190](https://connect.microsoft.com/SQLServer/feedback/details/2845190) et [Article Connect 3123864](https://connect.microsoft.com/SQLServer/feedback/details/3123864).
- Profiler : 
   - Correction du problème qui empêchait Profiler de fonctionner lors du ciblage de SQL 2005.
   - Correction du problème où Profiler ne respectait pas l’option de connexion « Faire confiance au certificat de serveur ».
- Moniteur d’activité : correction d’un problème qui empêchait le Moniteur d’activité de fonctionner quand il ciblait une instance SQL Server exécutée sur Linux.
- Correction d’un problème avec la classe de transfert SMO qui ne transférait pas les objets source de données externe ou les objets format de fichier externe. Les objets de ces types sont maintenant inclus dans le transfert.
- Serveurs inscrits :
   - Activation de la requête multiserveur pour les serveurs de l’agent utilisateur (elle essaie d’utiliser le même jeton pour chaque serveur de l’agent utilisateur dans le groupe).
- Authentification universelle AD :
   - Correction du problème de non-prise en charge de l’authentification Azure AD.
   - Correction d’un problème qui empêchait le concepteur de tables ou de vues de fonctionner.
   - Correction d’un problème qui empêchait le fonctionnement des commandes « Sélectionner les 1 000 premières lignes » et « Modifier les 200 premières lignes ».
- Restauration de base de données : correction d’un problème où la restauration ne prenait pas en compte le dernier dossier du chemin d’accès lors du déplacement de fichiers vers un autre emplacement.
- Assistant Compression :
   - Correction d’un problème avec l’Assistant Gestion de la compression pour les index. Correction du problème empêchant les Assistants Compression des données de fonctionner sur SQL 2016 et versions antérieures.
        https://connect.microsoft.com/SQLServer/feedback/details/3139342
   - Ajout d’un Assistant Compression pour les tables et index Azure.
- Plan d’exécution de requêtes : 
   - Correction d’un problème où les opérateurs PDW n’étaient pas reconnus.
- Propriétés du serveur :
   - Correction d’un problème qui empêchait la modification de l’affinité des processeurs de serveur.


**Analysis Services (AS)**

- Correction de plusieurs problèmes avec l’Assistant Déploiement pour prendre en charge les modèles tabulaires de niveau de compatibilité 1400 et les sources de données Power Query.
- L’Assistant Déploiement peut maintenant être déployé sur Azure AS lors de l’exécution à partir de la ligne de commande.
- S’il a choisi l’authentification Windows dans Azure AS, l’utilisateur voit désormais le nom de son compte d’utilisateur correctement affiché dans l’Explorateur d’objets.


### <a name="known-issues-in-this-173-release"></a>Problèmes connus dans cette version 17.3 :

**SSMS général**

- Les fonctionnalités SSMS suivantes ne sont pas prises en charge pour l’authentification Azure AD qui utilise l’agent utilisateur avec MFA :
   - L’Assistant Paramétrage du moteur de base de données n’est pas pris en charge pour l’authentification Azure AD. Il existe un problème connu où le message d’erreur présenté à l’utilisateur n’est pas très clair : « Impossible de charger le fichier ou l’assembly 'Microsoft.IdentityModel.Clients.ActiveDirectory,... » au lieu du message attendu : « L’Assistant Paramétrage du moteur de base de données ne prend pas en charge Microsoft Azure SQL Database. (DTAClient) ».
- La tentative d’analyse d’une requête dans les résultats DTA provoque une erreur : « L’objet doit implémenter IConvertible. mscorlib ».
- L’option *Requêtes régressées* ne figure pas dans la liste Magasin des requêtes des rapports affichés dans l’Explorateur d’objets.
   - Solution de contournement : cliquez avec le bouton droit sur le nœud **Magasin des requêtes** et sélectionnez **Afficher les requêtes régressées**.

**Integration Services (IS)**

- Le paramètre [execution_path] dans [catalog].[event_message] n’est pas correct pour les exécutions de package dans Scale Out. [execution_path] commence par « \Package » au lieu du nom d’objet du package exécutable. Quand vous affichez le rapport Vue d’ensemble des exécutions de package dans SSMS, le lien « Chemin d’accès de l’exécution » dans Vue d’ensemble de l’exécution ne fonctionne pas. Solution de contournement : cliquez sur « Afficher les messages » dans le rapport Vue d’ensemble pour vérifier tous les messages d’événement.


## <a name="downloadssdtmediadownloadpng-ssms-172httpsgomicrosoftcomfwlinklinkid854085"></a>![télécharger](../ssdt/media/download.png) [SSMS 17.2](https://go.microsoft.com/fwlink/?linkid=854085)
Disponibilité générale | Numéro de build : 14.0.17177.0

[Chinois (République populaire de Chine)](https://go.microsoft.com/fwlink/?linkid=854085&clcid=0x804) | [Chinois (Taïwan)](https://go.microsoft.com/fwlink/?linkid=854085&clcid=0x404) | [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=854085&clcid=0x409) | [Français](https://go.microsoft.com/fwlink/?linkid=854085&clcid=0x40c) | [Allemand | ](https://go.microsoft.com/fwlink/?linkid=854085&clcid=0x407)[Italien](https://go.microsoft.com/fwlink/?linkid=854085&clcid=0x410) | [Japonais](https://go.microsoft.com/fwlink/?linkid=854085&clcid=0x411) | [Coréen](https://go.microsoft.com/fwlink/?linkid=854085&clcid=0x412) | [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=854085&clcid=0x416) | [Russe](https://go.microsoft.com/fwlink/?linkid=854085&clcid=0x419) | [Espagnol](https://go.microsoft.com/fwlink/?linkid=854085&clcid=0x40a)

### <a name="enhancements"></a>Améliorations

- L’authentification multifacteur (MFA)
  - L’authentification de plusieurs utilisateurs Azure AD pour une authentification universelle avec l’authentification multifacteur (agent utilisateur avec MFA)
  - Un nouveau champ d’entrée des informations d’identification de l’utilisateur a été ajouté pour l’authentification universelle avec MFA afin de prendre en charge l’authentification de plusieurs utilisateurs.
- La boîte de dialogue de connexion prend désormais en charge les 5 méthodes d’authentification suivantes :
  - Authentification Windows
  - Authentification SQL Server
  - Active Directory - Authentification universelle avec prise en charge de MFA
  - Active Directory - Authentification par mot de passe
  - Active Directory - Authentification intégrée

- L’Assistant Importation/exportation de base de données pour DacFx utilisant l’authentification universelle avec MFA.
- Pour la prise en charge de l’API, consultez [IUniversalAuthProvider Interface](https://msdn.microsoft.com/library/microsoft.sqlserver.dac.iuniversalauthprovider.aspx) (Interface IUniversalAuthProvider).
- La bibliothèque ADAL gérée, utilisée par l’authentification universelle Azure AD avec MFA, a été mise à niveau vers la version 3.13.9.
- Par ailleurs, une nouvelle interface CLI a été publiée, prenant en charge le paramètre d’administration Azure AD pour SQL Database et SQL Data Warehouse.

 Pour plus d’informations sur les méthodes d’authentification Active Directory, consultez [Authentification universelle avec SQL Database et SQL Data Warehouse (prise en charge de SSMS pour MFA)](https://docs.microsoft.com/azure/sql-database/sql-database-ssms-mfa-authentication) et [Configurer l’authentification multifacteur Azure SQL Database pour SQL Server Management Studio](https://docs.microsoft.com/azure/sql-database/sql-database-ssms-mfa-authentication-configure).

- La fenêtre Sortie a des entrées pour les requêtes exécutées pendant le développement des nœuds de l’Explorateur d’objets
- Concepteur de vues activé pour les bases de données Azure SQL Database
- Les options de script par défaut pour les objets de script dans l’Explorateur d’objets de SSMS ont changé :
  - Avant, la valeur par défaut pour une nouvelle installation spécifiait que le script généré devait cibler la dernière version de SQL Server (actuellement SQL Server 2017).
  - Dans SSMS 17.2, une nouvelle option a été ajoutée : *Faire correspondre les paramètres de script à la source*. Quand la valeur est définie sur *True*, le script généré cible les mêmes version, type de moteur et édition de moteur que le serveur à partir duquel l’objet est scripté.
  - Comme la valeur de *Faire correspondre les paramètres de script à la source* est définie sur *True* par défaut, les nouvelles installations de SSMS scriptent toujours automatiquement par défaut les objets sur la même cible que celle du serveur d’origine.
  - Quand la valeur de *Faire correspondre les paramètres de script à la source* est définie sur *False*, les options de cible de script normales sont activées et fonctionnent comme avant.
    - Par ailleurs, toutes les options de script ont été déplacées dans leur propre section - *Options de version*. Elles ne sont plus sous *Options de script générales*.

- Nouvelle prise en charge des clouds nationaux dans « Restaurer à partir de l’URL »
- Les rapports QueryStoreUI prennent désormais en charge des métriques supplémentaires (RowCount, DOP, CLR Time, etc.) de sys.query_store_runtime_stats.
- IntelliSense est maintenant pris en charge par Azure SQL Database
    - https://connect.microsoft.com/SQLServer/feedback/details/3100677/ssms-2016-would-be-nice-to-have-intellisense-on-azure-sql-databases
- Sécurité : par défaut, la boîte de dialogue de connexion ne fait pas confiance aux certificats de serveur et demande le chiffrement des connexions Azure SQL Database
- Améliorations générales de la prise en charge de SQL Server sur Linux :
 - Le nœud Messagerie de base de données est de retour
 - Différents problèmes concernant les chemins ont été traités
 - Le moniteur d’activité est plus stable
 - La boîte de dialogue Propriétés de connexion affiche la plateforme appropriée
- Le rapport de serveur Tableau de bord des performances est désormais disponible comme rapport par défaut :
  - Peut se connecter à SQL Server 2008 et versions ultérieures.
  - Le sous-rapport Index manquants utilise la notation pour permettre d’identifier les index plus utiles.
  - Le sous-rapport Historique des statistiques d’attente regroupe maintenant les attentes par catégorie. Les attentes d’inactivité et de mise en veille sont filtrées par défaut.
  - Nouveau sous-rapport Historique des verrous.
- La recherche de nœuds de plan d’exécution de requêtes permet d’effectuer des recherches dans les propriétés de plan. Recherchez facilement n’importe quelle propriété d’opérateur, comme un nom de table. Pour utiliser cette option quand vous affichez un plan :
  - Cliquez avec le bouton droit sur le plan et, dans le menu contextuel, cliquez sur l’option Rechercher un nœud
  - Utiliser Ctrl+F


**Analysis Services (AS)**

- Une nouvelle sélection de membre du rôle AAD a été ajoutée pour les utilisateurs sans adresse e-mail dans les modèles Azure AS dans SSMS

**Integration Services (IS)**

- Ajout d’une nouvelle colonne (« Nombre d’exécutions ») dans le rapport d’exécution de SSIS

### <a name="known-issues-in-this-release"></a>Problèmes connus dans cette version :

- Les fenêtres de requête qui utilisent l’authentification « Active Directory – Authentification universelle avec prise en charge de MFA » peuvent rencontrer une erreur similaire à la suivante, quand vous tentez d’exécuter une requête ouverte depuis une heure :

   `Msg 0, Level 11, State 0, Line 0
The connection is broken and recovery is not possible. The client driver attempted to recover the connection one or more times and all attempts failed. Increase the value of ConnectRetryCount to increase the number of recovery attempts.`

   La réexécution de la requête doit permettre de surmonter l’erreur.

- Les fonctionnalités SSMS suivantes ne sont pas prises en charge pour l’authentification Azure AD qui utilise l’authentification universelle avec MFA :
  - Le concepteur **Nouvelle table/vue** affiche l’ancienne invite de connexion et ne fonctionne pas pour l’authentification Azure AD.
  - La fonctionnalité **Modifier les 200 premières lignes** ne prend pas en charge l’authentification Azure AD.
  - Le composant **Serveur inscrit** ne prend pas en charge l’authentification Azure AD.
  - L’**Assistant Paramétrage du moteur de base de données** n’est pas pris en charge pour l’authentification Azure AD. Il existe un problème connu où le message d’erreur présenté à l’utilisateur est loin d’être utile : *Impossible de charger le fichier ou l’assembly ’Microsoft.IdentityModel.Clients.ActiveDirectory,...* au lieu du message attendu : *L’Assistant Paramétrage du moteur de base de données ne prend pas en charge Microsoft Azure SQL Database. (DTAClient)* .

**Analysis Services (AS)**

- L’Explorateur d’objets dans SSAS n’affiche pas le nom d’utilisateur de l’authentification Windows dans les propriétés de connexion d’Azure AS.

### <a name="bug-fixes"></a>Correctifs de bogues

- Correction d’un problème quand vous tentez d’imprimer les résultats d’une requête (sous forme de texte).  https://connect.microsoft.com/SQLServer/feedback/details/3055225/
- Correction d’un problème où SSMS supprimait sans raison des tables et d’autres objets lors de la mise en script de la suppression de ces objets sur une base de données SQL Azure.
- Correction d’un problème où SSMS refusait parfois de démarrer avec une erreur de type « Un ou plusieurs composants introuvables. Réinstallez l’application »
- Correction d’un problème où le SPID dans l’interface utilisateur de SSMS pouvait devenir obsolète et ne plus être synchronisé. https://connect.microsoft.com/SQLServer/feedback/details/1898875
- Correction d’un problème dans le programme d’installation (sans assistance) de SSMS où l’argument /passive était traité comme /quiet.
- Correction d’un problème où SSMS levait parfois une erreur « Référence d’objet non définie sur une instance de l’objet » au démarrage. http://connect.microsoft.com/SQLServer/feedback/details/3134698
- Correction d’un problème avec l’« Assistant Compression de données » qui était à l’origine du blocage de SSMS quand vous appuyiez sur « Calculer » sur la table graphique
- Correction d’un problème de performances quand vous cliquiez avec le bouton droit sur l’index d’une table (avec une connexion Internet lente). https://connect.microsoft.com/SQLServer/feedback/details/3120783
- Correction d’un problème où SSMS ne pouvait pas énumérer les fichiers de sauvegarde sur les serveurs avec un classement respectant la casse. http://connect.microsoft.com/SQLServer/feedback/details/3134787 et https://connect.microsoft.com/SQLServer/feedback/details/3137000
- Corrections assorties pour le plan d’exécution de requêtes et la comparaison de plans d’exécution de requêtes
- Correction d’un problème où la boîte de dialogue Connexion ne permettait pas à l’utilisateur de spécifier le « Protocole réseau » à utiliser pour la connexion, à moins que SQL Server ne soit installé sur l’ordinateur exécutant SSMS. https://connect.microsoft.com/SQLServer/feedback/details/3134997
- Amélioration de la prise en charge des configurations multimoniteurs où des boîtes de dialogue SSMS s’affichaient dans des emplacements « aléatoires ». Ajout d’une nouvelle option « Boîtes de dialogue de tâches » sous les paramètres utilisateur « Explorateur d’objets SQL Server | Commandes » pour permettre la mémorisation de la position d’une boîte de dialogue de tâche ou d’une feuille de propriétés quand elle se ferme. https://connect.microsoft.com/SQLServer/feedback/details/889169, https://connect.microsoft.com/SQLServer/feedback/details/1158271, https://connect.microsoft.com/SQLServer/feedback/details/3135260 
- Correction d’un problème où SSMS ne pouvait pas changer les propriétés d’une base de données Azure SQL Database chiffrée
- Amélioration de l’option « Ignorer les résultats après l’exécution ». https://connect.microsoft.com/SQLServer/feedback/details/1196581
- Amélioration/correction d’un problème où les utilisateurs ne pouvaient pas accéder aux abonnements Azure pour lesquels ils n’étaient pas administrateurs.
- Amélioration de l’Assistant « Restauration de la base de données » pour que la base de données cible reste sélectionnée dans l’Explorateur d’objets, indépendamment de la sélection de la base de données source. https://connect.microsoft.com/SQLServer/feedback/details/3118581
- Correction d’un problème où l’Explorateur d’objets ne triait pas correctement les « Procédures stockées compilées en mode natif » récemment ajoutées. http://connect.microsoft.com/SQLServer/feedback/details/3133365
- Correction d’un problème où « SÉLECTIONNER LES N PREMIÈRES LIGNES » n’incluait pas la clause « TOP ». Pour Azure SQLDW. https://connect.microsoft.com/SQLServer/feedback/details/3133551 et https://connect.microsoft.com/SQLServer/feedback/details/3135874
- QueryStoreUI : Correction d’un problème où les intervalles de temps non personnalisés ne fonctionnaient pas correctement pour tous les rapports.
- Always Encrypted :
    - Amélioration de la messagerie pour l’état de l’autorisation AKV dans la boîte de dialogue Nouvelle clé CMK
    - Ajout d’info-bulles à la liste déroulante des clés CEK pour faciliter la distinction des clés CEK avec des noms longs
    - Correction d’un problème où certains fournisseurs de magasin de clés CNG n’étaient pas affichés dans la boîte de dialogue Nouvelle clé principale de colonne pour Always Encrypted
- Correction du « Nom d’application » incohérent pour les connexions SSMS. http://connect.microsoft.com/SQLServer/feedback/details/3135115
- Correction d’un problème où SSMS ne générait pas de scripts corrects pour SQL Azure (tables et index avec l’option DATA_COMPRESSIONS). https://connect.microsoft.com/SQLServer/feedback/details/3133148
- Correction d’un problème où l’utilisateur ne pouvait pas utiliser le raccourci CTRL+Q pour le Lancement rapide (Remarque : Les nouvelles combinaisons de touches pour activer/désactiver l’option « IntelliSense activé » dans l’éditeur de requête sont désormais CTRL+B, CTRL+I.) https://connect.microsoft.com/SQLServer/feedback/details/3131968
- Correction d’un problème dans la « Restauration de base de données » où SSMS levait une exception au moment de sélectionner un compte de stockage à partir d’un abonnement avec des comptes associés à des domaines personnalisés définis
- Correction d’un problème dans « Diagramme de base de données » où SSMS levait une erreur « L’index était en dehors des limites du tableau ». Par ailleurs, l’utilisateur pouvait seulement changer la « vue Table » en vue standard. https://connect.microsoft.com/SQLServer/feedback/details/3133792 et http://connect.microsoft.com/SQLServer/feedback/details/3135326
- Correction d’un problème dans « Sauvegarde/restauration vers l’URL » où SSMS n’énumérait pas les comptes de stockage classiques.
- Correction d’un problème où une exception était levée lors de la tentative d’ajout d’éléments sécurisables liés à un schéma à des rôles de base de données. https://connect.microsoft.com/SQLServer/feedback/details/3118143
- Correction d’un problème où SSMS affichait par intermittence l’erreur « Les données sont de type Null. Cette méthode ou propriété ne peut pas être appelée sur des valeurs Null. » lors du développement d’un nœud de table http://connect.microsoft.com/SQLServer/feedback/details/3136283
- DTA : Correction d’un problème où DTAEngine.exe se terminait avec une altération du tas lors de l’évaluation de la fonction de partition avec certaines valeurs limites.


**Analysis Services (AS)**

- Correction d’un problème où la commande AS Restaurer la base de données échouait avec une erreur si la base de données avait un nom différent de l’ID
- Correction d’un problème où la fenêtre de requête DAX ignorait l’option de menu permettant d’activer/désactiver IntelliSense activé
- Correction d’un problème qui empêchait de se connecter à SSAS avec des adresses http/https msmdpump IIS
- Autorisation de la connexion à Azure AS à l’aide d’un mot de passe contenant un point-virgule
- Le script de la commande AS Restaurer la base de données avec l’option « Ignorer l’appartenance » inclut la nouvelle option JSON correspondante quand il est utilisé avec le serveur SQL Server 2017 AS ou Azure AS
- Correction d’un problème très rare où la boîte de dialogue de suppression de base de données levait une erreur lors du chargement
- Correction d’un problème qui pouvait se produire quand vous tentiez d’afficher des partitions dans le modèle de niveau de compatibilité 1400 contenant un mélange de définitions de requête SQL et de partition M

**Integration Services (IS)**
- Correction d’un problème où il était impossible d’afficher les rapports d’informations sur l’exécution du catalogue SSISDB
- Correction de problèmes dans SSMS relatifs aux performances médiocres liées à un grand nombre de projets/packages

## <a name="downloadssdtmediadownloadpng-ssms-171httpsgomicrosoftcomfwlinklinkid849819"></a>![télécharger](../ssdt/media/download.png) [SSMS 17.1](https://go.microsoft.com/fwlink/?linkid=849819)
Disponibilité générale | Numéro de build : 14.0.17119.0

[Chinois (République populaire de Chine)](https://go.microsoft.com/fwlink/?linkid=849819&clcid=0x804) | [Chinois (Taïwan)](https://go.microsoft.com/fwlink/?linkid=849819&clcid=0x404) | [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=849819&clcid=0x409) | [Français](https://go.microsoft.com/fwlink/?linkid=849819&clcid=0x40c) | [Allemand | ](https://go.microsoft.com/fwlink/?linkid=849819&clcid=0x407)[Italien](https://go.microsoft.com/fwlink/?linkid=849819&clcid=0x410) | [Japonais](https://go.microsoft.com/fwlink/?linkid=849819&clcid=0x411) | [Coréen](https://go.microsoft.com/fwlink/?linkid=849819&clcid=0x412) | [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=849819&clcid=0x416) | [Russe](https://go.microsoft.com/fwlink/?linkid=849819&clcid=0x419) | [Espagnol](https://go.microsoft.com/fwlink/?linkid=849819&clcid=0x40a)

### <a name="enhancements"></a>Améliorations

- Profileur : Aide > À propos de affiche maintenant le numéro de version (par exemple 17.1)
- Les utilisateurs Analysis Service peuvent actualiser les informations d’identification de leurs sources de données pour les modèles 1200 TM et supérieur dans le menu contextuel sur la source de données.
- Les rapports SSIS prédéfinis montrent maintenant les journaux de l’exécution de SSIS Scale Out dans CTP 2.1
- Application de gestion de SSIS Scale Out
  - Affichez les informations de base de Scale Out Master.
  - Ajoutez facilement un worker au déploiement de Scale Out.
  - Affichez tous les Scale Out workers et les informations de base les concernant. Vous pouvez aussi les activer ou les désactiver facilement.

### <a name="bug-fixes"></a>Correctifs de bogues
- AlwaysOn :
  - Correction d’un problème où les propriétés d’un réplica de disponibilité étaient toujours affichées avec le mode « Basculement automatique » pour les groupes de disponibilité WSFC.
  - Correction d’un problème où la liste de routage en lecture seule était remplacée lors de la mise à jour du groupe de disponibilité
- Always Encrypted : correction d’un problème où des informations générées par DacFx étaient manquantes dans le fichier journal généré.
- ShowPlan : correction d’un problème où l’interface utilisateur affichait toujours l’attribut de type de jointure réel pour les opérateurs de jointure non adaptatifs.
- Programme d’installation :
  - Correction d’un problème où SSMS 17.0 endommageait SSDT sur Visual Studio 2013 [Article de Microsoft Connect 3133479]
  - Correction d’un problème où cliquer sur « Redémarrer » à la fin du programme d’installation ne redémarrait pas la machine
- Script : empêchement temporaire de la suppression accidentelle d’objets de base de données Azure par SSMS lors de la création d’un script de suppression, via la désactivation de cette option.  Un correctif approprié sera apporté dans une prochaine version de SSMS.
- Explorateur d’objets : correction d’un problème où le nœud « Bases de données » n’était pas développé lors de la connexion à une base de données Azure créée avec « AS COPY »

## <a name="downloadssdtmediadownloadpng-ssms-170httpgomicrosoftcomfwlinklinkid847722"></a>![télécharger](../ssdt/media/download.png) [SSMS 17.0](http://go.microsoft.com/fwlink/?LinkID=847722)
Disponibilité générale | Numéro de build : 14.0.17099.0

[Chinois (République populaire de Chine)](https://go.microsoft.com/fwlink/?linkid=847722&clcid=0x804) | [Chinois (Taïwan)](https://go.microsoft.com/fwlink/?linkid=847722&clcid=0x404) | [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=847722&clcid=0x409) | [Français](https://go.microsoft.com/fwlink/?linkid=847722&clcid=0x40c) | [Allemand | ](https://go.microsoft.com/fwlink/?linkid=847722&clcid=0x407)[Italien](https://go.microsoft.com/fwlink/?linkid=847722&clcid=0x410) | [Japonais](https://go.microsoft.com/fwlink/?linkid=847722&clcid=0x411) | [Coréen](https://go.microsoft.com/fwlink/?linkid=847722&clcid=0x412) | [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=847722&clcid=0x416) | [Russe](https://go.microsoft.com/fwlink/?linkid=847722&clcid=0x419) | [Espagnol](https://go.microsoft.com/fwlink/?linkid=847722&clcid=0x40a)

### <a name="enhancements"></a>Améliorations 

- Package de mise à niveau et Windows Software Update Services (WSUS) 
    - Les versions futures 17.X incluront un package de mises à jour cumulatives plus petit 
  - Le package de mise à jour sera également publié sur le catalogue WSUS  
- Mises à jour des icônes
    - Des icônes ont été mises à jour pour être cohérentes avec les icônes fournies par le Shell Visual Studio. Elles prennent en charge des résolutions élevées.
    - Nouvelles icônes de programme SSMS et Profiler permettant de différencier les versions 16.X et 17.X
- Module SQL PowerShell
  - Le module PowerShell SQL Server a été supprimé de SSMS et est désormais fourni via PowerShell Gallery (PowerShell 5.0 est maintenant obligatoire pour la prise en charge de la gestion des versions des modules)
  - Améliorations diverses à la « présentation » (mise en forme) de certains objets SMO (par exemple, les bases de données indiquent désormais la taille et la quantité d’espace disponible, et les tables indiquent le nombre de lignes et l’utilisation de l’espace)
  - Colorisation ajoutée quand l’invite de commandes PowerShell est appelée à partir du menu « Démarrer PowerShell » dans l’Explorateur d’objets
  - Les paramètres ClusterType et -RequiredCopiesToCommit ont été ajoutés aux applets de commande AG (New-SqlAvailabilityGroup, Join-SqlAvailabilityGroup, et les applets de commande Set-SqlAvailabilityGroup)
  - Les paramètres - ActiveDirectoryAuthority et - AzureKeyVaultResourceId ont été ajoutés à l’applet de commande Add-SqlAzureAuthenticationContext
  - Ajout des applets de commande Revoke-SqlAvailabilityGroupCreateAnyDatabase, Grant-SqlAvailabilityGroupCreateAnyDatabase et Set-SqlAvailabilityReplicaRoleToSecondary
  - Ajout du paramètre SeedingMode aux applets de commande Set-SqlAvailabilityReplica et New-SqlAvailabilityReplica
  - Ajout du paramètre -ConnectionString à Get-SqlDatabase
- SQL Server sur Linux
    - Améliorations d’ordre général et correctifs pour la copie des journaux de transaction
  - Ajout de la prise en charge des chemins Linux natifs pour attacher, sauvegarder et restaurer une base de données
  - Ajout de la prise en charge des chemins Linux natifs pour le dossier de destination du journal d’audit
- Analysis Services
  - Fenêtre de requête DAX :
    - Mise en correspondance des parenthèses dans l’éditeur
    - Prise en charge de la syntaxe DEFINE MEASURE et DEFINE VAR
    - Différentes améliorations apportées à Intellisense
  - Authentification universelle
    - Permet aux utilisateurs de spécifier un nom d’utilisateur et aucun mot de passe : la boîte de dialogue de connexion d’Azure gère alors la connexion
  - Intégration de PQ SSMS : 
    - Écriture de scripts de travaux de sources de données structurées 
    - Affichage et modification de sources de données structurées dans l’interface utilisateur de PQ
- Nouveau modèle « Ajouter une contrainte Unique »
- Showplan
    - Affichage du maximum au lieu de la somme pour les threads dans la fenêtre Propriétés
    - Affichage des propriétés du nouvel opérateur d’allocation de mémoire
    - Activation du bouton Modifier la requête dans les statistiques des requêtes dynamiques
    - Prise en charge de l’exécution entrelacée
  - Nouvelle option pour « Analyser le plan d’exécution réel »
  - Améliorations générales apportées à la comparaison du plan d’exécution de requêtes
  - Une fonction a été introduite dans la fonctionnalité de comparaison de plan d’exécution de requêtes pour rechercher des différences significatives dans l’estimation de la cardinalité entre les nœuds correspondants de deux plans de requête et pour effectuer une analyse des causes principales possibles.
- Suppression du gestionnaire de configuration à partir de l’Explorateur de serveurs inscrits
- Activation de la lecture des journaux d’audit à partir du stockage d’objets blob Azure
- Un paramétrage a été ajouté pour Always Encrypted. Reportez-vous à [cette page](https://blogs.msdn.microsoft.com/sqlsecurity/2016/12/13/parameterization-for-always-encrypted-using-ssms-to-insert-into-update-and-filter-by-encrypted-columns/) pour plus de détails 
- La connexion d’authentification universelle AAD à Azure SQL Database prend en charge les ID de locataire personnalisés 
- La génération de scripts pour Azure SQL Database inclut désormais le texte intégral, les règles et la base de données
- Correctifs de la personnalisation dans les écrans de démarrage pour SSMS et le profileur
- Suppression dans SSMS de l’interface utilisateur du point de contrôle de l’utilitaire
- SSMS peut maintenant créer des bases de données SQL Azure dans l’édition « PremiumRS »
- Groupes de disponibilité Always On
  - Ajout de la prise en charge pour de nouveaux types de cluster : EXTERNAL et NONE
    - Ajout de la prise en charge de SQL Server sur Linux
    - Ajout de l’amorçage automatique comme option pour la synchronisation initiale des données
    - Correction de certains problèmes, par exemple la gestion des URL des points de terminaison, l’actualisation des bases de données et la présentation de l’interface utilisateur
    - Suppression de fonctionnalités relatives aux réplicas Azure
  - Amélioration d’IntelliSense pour plusieurs mots clés des groupes de disponibilité
- Moniteur d'activité
  - Ajout d’un nouveau volet « Moniteur d’activité » à la fenêtre de sortie de SSMS
  - Modification du message d’erreur/de dépassement du délai d’expiration de connexion pour consigner des informations dans la fenêtre de sortie au lieu d’afficher un message contextuel
  - Suppression du graphique vide (graphique 5) dans la section Vue d’ensemble
  - Ajout de « (en pause) » au titre Vue d’ensemble si la collecte de données du moniteur d’activité est mise en pause
  - Extensions de graphe SQL Server 
    - Nouvelles icônes pour les tables de nœuds et d’arêtes de graphe
    - Les tables de nœuds et d’arêtes de graphe sont affichées sous le dossier Tables graphiques
    - Disponibilité de modèles pour créer des tables de nœuds et d’arêtes de graphe
- Mode de présentation
    - 3 nouvelles tâches disponibles via le menu de lancement rapide (CTRL-Q)
    - PresentOn - Activation du mode de présentation
    - PresentEdit - Modification de la taille des polices pour le mode de présentation.  « Police Éditeur de texte » pour l’éditeur de requête.  « Police Environnement » pour les autres composants.
    - RestoreDefaultFonts - Rétablissement des paramètres par défaut.
    - *Remarque : il n’existe actuellement aucune commande PresentOff pour l’instant.  Utiliser RestoreDefaultFonts pour désactiver le mode de présentation*

### <a name="bug-fixes"></a>Correctifs de bogues

- Correction d’un problème où SSMS se bloquait quand l’utilisateur faisait défiler le plan d’exécution de requêtes via le pavé tactile d’un Surface Book
- Correction d’un problème où SSMS se bloquait pendant un long moment lors de l’obtention des propriétés d’une base de données en cours de restauration ou hors connexion 
- Correction d’un problème où la « Visionneuse d’aide » ne pouvait pas être ouverte dans les builds RC
- Correction d’un problème où les éléments de la « Boîte à outils des tâches des plans de maintenance » peuvent être manquants dans SSMS.
- Correction d’un problème dans SSMS où l’utilisateur ne pouvait pas réduire une base de données quand le nom de cette base de données contenait des accolades. [Article de Microsoft Connect](https://connect.microsoft.com/SQLServer/feedback/details/3122618)
- Correction d’un problème où SSMS, au moment où il essayait de créer un script pour supprimer une base de données Azure, provoquait la suppression de la base de données elle-même. [Article de Microsoft Connect](http://connect.microsoft.com/SQLServer/feedback/details/3131458/)
- Correction d’un problème où les valeurs par défaut n’ont pas fait l’objet d’un script pour les types de tables définis par l’utilisateur. [Article de Microsoft Connect](https://connect.microsoft.com/SQLServer/feedback/details/3119027)
- Autre série d’améliorations des performances liées au menu contextuel des index. [Article de Microsoft Connect](https://connect.microsoft.com/SQLServer/feedback/details/3120783)
- Correction du problème qui provoquait un scintillement excessif lorsque des index manquants du plan d’exécution faisaient l’objet d’un pointage de la souris. [Article de Microsoft Connect](https://connect.microsoft.com/SQLServer/feedback/details/3118510)
- Correction d’un problème où SSMS prenait la base de données hors connexion lors de l’écriture de scripts [Article de Microsoft Connect](https://connect.microsoft.com/SQLServer/feedback/details/3118550)
- Divers correctifs liés à l’interface utilisateur sur les versions localisées (dans une langue que l’anglais) de SSMS.
- Correction du problème où le nœud de « Always Encrypted Keys » était manquant lors du ciblage de SQL 2016 SP1 Édition Standard.
- Always Encrypted
    - Activation incorrecte du menu Always Encrypted lors du ciblage de SQL 2016 RTM Édition Standard ou de n’importe quel serveur SQL 2014 (et versions antérieures)
    - Correction d’un problème où IntelliSense signale une erreur lors de l’utilisation de la syntaxe CREATE ou ALTER
    - Correction du problème où le chiffrement échoue si CMK/CEK contient des caractères qui devraient être ignorés, par exemple mis entre crochets
    - En cas d’exception liée à une mémoire insuffisante dans SSMS, l’utilisateur reçoit une erreur proposant d’utiliser le module PowerShell (64 bits) natif.
    - Correction du problème lié à l’échec de l’Assistant AE si l’utilisateur a utilisé des abonnements de responsable de groupe de ressources au lieu d’abonnements Azure classiques
    - Correction du problème où l’Assistant AE affiche une erreur incorrecte lorsque l’utilisateur ne détient aucune autorisation dans le cadre de ces abonnements ni aucun coffre de clé Azure.
    - Correction d’un problème de l’Assistant AE où la page de connexion du coffre de clé Azure n’affiche pas les abonnements Azure en présence de plusieurs AAD
    - Correction d’un problème de l’Assistant AE où la page de connexion du coffre de clé Azure n’affiche pas les abonnements Azure pour lesquels l’utilisateur détient une autorisation de lecture
  - Correction d’un problème où les fichiers de ressources ne pouvaient pas être chargés correctement, ce qui provoquait des messages d’erreur imprécis
- Amélioration du contraste des liens hypertexte sur la page Installation de SSMS
- Correction d’un problème lié au non-affichage des nœuds Polybase lors de la connexion à SQL Server Express (2016 SP1)
- Correction d’un problème où SSMS ne pouvait pas changer le niveau de compatibilité d’une base de données Azure pour le faire passer à v140
- Amélioration des performances de l’Explorateur d’objets lors du développement de la liste des bases de données Azure [Article de Microsoft Connect](https://connect.microsoft.com/SQLServer/feedback/details/3100675)
- Correction d’un problème où l’élément de menu contextuel Afficher le journal SQL Server apparaît de manière incorrecte pour les types de serveurs non relationnels (AS\RS\IS) 
- Correction d’un problème dans lequel la vérification de la syntaxe d’une requête de partition Analysis Services à l’aide de l’authentification SQL pouvait entraîner un message d’échec de connexion
- Correction d’un problème dans lequel le fait de renommer un modèle tabulaire AS de niveau de compatibilité 1400 d’aperçu pouvait échouer dans SSMS
- Correction d’un problème lié au message « échec de l’opération sur le modèle » qui pouvait se produire après avoir tenté une opération non valide sur le serveur AS dans de rares circonstances et rétablir les modifications locales suite à un échec d’enregistrement sur le modèle
- Correction d’une faute d’orthographe dans la boîte de dialogue contextuelle de synchronisation des bases de données Analysis Services
- Les boîtes de dialogue de sauvegarde/restauration de conteneur se trouvent en dehors de l’écran dans les configurations avec plusieurs moniteurs. 
- La création d’une stratégie de sécurité échoue si le nom de l’objet cible contient le caractère « ] ».
- Le menu « Ouvrir un fichier récent » de SSMS 2016 n’affiche pas les fichiers récemment enregistrés. [Article de Microsoft Connect](https://connect.microsoft.com/SQLServer/feedback/details/3113288/ssms-2016-open-recent-menu-doesnt-show-recently-saved-files)
- Suppression de la réinitialisation des paramètres utilisateur lors de la mise à jour du shell VS.
- Correction d’un problème qui empêchait l’utilisateur de pouvoir changer le niveau de compatibilité d’une base de données sur SQL Server 2017.
- Les fenêtres de requête utilisant l’authentification universelle AAD ne peuvent pas actualiser la requête au bout d’une heure.
- Suppression dans SSMS de l’interface utilisateur du point de contrôle de l’utilitaire.
- Les connexions d’authentification universelle AAD échouent à interroger des données après l’expiration du jeton initial.
- Impossible de générer des scripts de règles depuis Azure SQL Database vers Azure SQL Database.
- Résolution d’un problème où PowerShell SQL ne pouvait pas se connecter à des instances SQL héritées (2014 et antérieur). [Article de Microsoft Connect](https://connect.microsoft.com/SQLServer/feedback/details/1138754/sql-server-sqlps-powershell-module-fails-connection-to-sql-2012-instance)
- Résolution d’un problème qui provoquait le blocage de SSMS lors d’un échec d’importation des serveurs inscrits.
- Résolution d’un problème qui provoquait le blocage de SSMS si un utilisateur avait certaines autorisations sur une base de données. 
- SSMS - des tables disparaissent de l’aire de conception pendant l’examen des vues. [Article de Microsoft Connect](https://connect.microsoft.com/SQLServer/feedback/details/2946125/ssms-tables-disappears-from-design-surface-while-reviewing-views) 
- La barre de défilement des tables ne permet pas à l’utilisateur de faire défiler le contenu d’une table : seule la flèche haut/bas le permet. Il est également possible de faire défiler le contenu de la table après avoir essayé de faire défiler à l’aide de la barre de défilement, ce qui est un bogue. [Article de Microsoft Connect](
http://connect.microsoft.com/SQLServer/feedback/details/3106561/sql-server-manager-2016-bug-in-design-view) 
- Les icônes ne s’affichent pas pour les serveurs inscrits une fois le nœud racine actualisé.
- Le bouton du script pour créer une base de données sur les serveurs v12 Azure exécute un script, puis affiche le message « Aucune action pour le script ».
- La boîte de dialogue SSMS Se connecter au serveur n’efface pas l’onglet « Propriétés supplémentaires » pour chaque nouvelle connexion.
- Le script de génération de tâches ne génère pas de scripts de création de base de données pour une base de données Azure SQL Database.
- La barre de défilement du Concepteur de vues apparaît désactivée.
- Les chemins de clés AVK d’Always Encrypted n’incluent pas les ID de version.
- Réduction du nombre de requêtes de l’édition du moteur dans la fenêtre de requête. [Article de Microsoft Connect](http://connect.microsoft.com/SQLServer/feedback/details/3113387)
- Les erreurs Always Encrypted provenant de l’actualisation des modules après chiffrement ne sont pas gérées correctement.
- Changement du délai d’expiration de la connexion par défaut pour OLTP et OLAP de 15 à 30 secondes, pour résoudre une classe d’échecs de connexion ignorés. 
- Correction d’un blocage dans SSMS lors du lancement d’un rapport personnalisé. [Article de Microsoft Connect](http://connect.microsoft.com/SQLServer/feedback/details/3118856)
- Résolution d’un problème où « Générer un Script... » échoue pour les bases de données SQL Azure.
- Correction de « Générer un script en tant que » et « Assistant Génération de scripts », qui n’ajoutaient pas de sauts de ligne supplémentaires lors de la génération de scripts pour des objets comme les procédures stockées. [Article de Microsoft Connect](http://connect.microsoft.com/SQLServer/feedback/details/3115850)
- Le fournisseur SQLAS PowerShell : Ajout de la propriété LastProcessed aux dossiers Dimension et MeasureGroup. [Article de Microsoft Connect](http://connect.microsoft.com/SQLServer/feedback/details/3111879)
- Statistiques sur les requêtes en direct : Résolution d’un problème où elles montraient seulement la première requête dans un lot. [Article de Microsoft Connect] (http://connect.microsoft.com/SQLServer/feedback/details/3114221)  
- Plan d’exécution de requêtes : Affichage du maximum au lieu de la somme pour les threads dans la fenêtre Propriétés.
- Magasin de requêtes : Ajout d’un nouveau rapport sur les requêtes avec les variation fortes des exécutions.
- Problèmes de performances de l’Explorateur d’objets : [Article de Microsoft Connect](http://connect.microsoft.com/SQLServer/feedback/details/3114074)
    - Le menu contextuel pour les tables se bloque momentanément
    - SSMS est lent quand l’utilisateur clique avec le bouton droit sur un index pour une table (via une connexion (Internet) à distance). 
    - Éviter d’émettre des requêtes sur des tables qui sont triées sur le serveur
- Suppression dans SSMS de l’Assistant Déploiement Azure (pour déployer une base de données sur une machine virtuelle Azure)
- Résolution d’un problème où les index manquants n’étaient pas affichés dans les plans d’exécution dans SSMS [Article de Microsoft Connect](http://connect.microsoft.com/SQLServer/feedback/details/3114194)
- Résolution du problème courant de blocage lors de l’arrêt dans SSMS
- Résolution du problème dans l’Explorateur d’objets, où une erreur se produisait lors de l’affichage du menu contextuel sur les nœuds Polybase | Groupe de scale-out[Article de Microsoft Connect](http://connect.microsoft.com/SQLServer/feedback/details/3115128)
- Résolution d’un problème où SSMS pouvait se bloquer lors d’une tentative d’affichage des autorisations sur une base de données
- Magasin de requêtes : améliorations générales apportées aux éléments du menu contextuel pour les grilles de résultats des rapports du magasin de requêtes
- La configuration d’Always Encrypted pour une table existante échoue avec des erreurs sur des objets non associés. [Article de Microsoft Connect](http://connect.microsoft.com/SQLServer/feedback/details/3103181)
- La configuration d’Always Encrypted pour une base de données existante avec plusieurs schémas ne fonctionne pas. [Article de Microsoft Connect] (http://connect.microsoft.com/SQLServer/feedback/details/3109591)
- L’Assistant Colonne chiffrée d’Always Encrypted échoue parce que la base de données contient des vues qui référencent des vues système. [Article de Microsoft Connect] (http://connect.microsoft.com/SQLServer/feedback/details/3111925)
- Lors du chiffrement à l’aide d’Always Encrypted, les erreurs provenant de l’actualisation des modules après le chiffrement ne sont pas gérées correctement.
- Résolution du problème de troncation de l’interface utilisateur dans la boîte de dialogue « Nouvelle inscription de serveur »
- Correction de la mise à jour incorrecte par l’interface utilisateur des conditions DMF pour les expressions contenant des valeurs de constante de chaîne incluant des guillemets
- Résolution d’un problème pouvant provoquer le blocage de SSMS lors de l’exécution de rapports personnalisés
- Ajout de l’élément de menu « Exécution dans Scale Out... » au nœud du dossier
- Correction d’un problème avec la fonctionnalité des adresses IP de la liste verte de pare-feu SQL Azure DB
- Correction d’un problème dans SSMS qui provoquait une exception de référence d’objet non définie lors de la modification de la source d’une partition multidimensionnelle AS
- Correction d’un problème dans SSMS qui provoquait une exception de référence d’objet non définie lors de la suppression d’un assembly de client d’un serveur AS multidimensionnel
- Correction d’un problème où le renommage d’une base de données 1400 tabulaire AS échouait
- Correction d’un problème de génération de script d’une source de données tabulaire AS de niveau de compatibilité 1400 à partir de la boîte de dialogue des propriétés de connexion
- Suppression de l’hypothèse selon laquelle les tables d’un modèle de niveau de compatibilité 1400 ont au moins une partition
- CTRL+R affiche/masque maintenant le volet de résultats dans l’éditeur de requête SSMS DAX


## <a name="downloadssdtmediadownloadpng-ssms-1653httpgomicrosoftcomfwlinklinkid840946"></a>![télécharger](../ssdt/media/download.png) [SSMS 16.5.3](http://go.microsoft.com/fwlink/?LinkID=840946)
Disponibilité générale | Numéro de build : 13.0.16106.4

[Chinois (République populaire de Chine)](https://go.microsoft.com/fwlink/?linkid=840946&clcid=0x804) | [Chinois (Taïwan)](https://go.microsoft.com/fwlink/?linkid=840946&clcid=0x404) | [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=840946&clcid=0x409) | [Français](https://go.microsoft.com/fwlink/?linkid=840946&clcid=0x40c) | [Allemand | ](https://go.microsoft.com/fwlink/?linkid=840946&clcid=0x407)[Italien](https://go.microsoft.com/fwlink/?linkid=840946&clcid=0x410) | [Japonais](https://go.microsoft.com/fwlink/?linkid=840946&clcid=0x411) | [Coréen](https://go.microsoft.com/fwlink/?linkid=840946&clcid=0x412) | [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=840946&clcid=0x416) | [Russe](https://go.microsoft.com/fwlink/?linkid=840946&clcid=0x419) | [Espagnol](https://go.microsoft.com/fwlink/?linkid=840946&clcid=0x40a)

Les problèmes suivants ont été résolus dans cette version :

* Résolution du problème introduit dans SSMS 16.5.2 qui provoquait l’expansion du nœud « Table » quand la table comportait plusieurs colonnes éparses.

* Les utilisateurs peuvent déployer des packages SSIS contenant le Gestionnaire de connexions OData qui connectent une ressource Microsoft Dynamics AX/CRM Online au catalogue SSIS. Pour plus d’informations, consultez [Gestionnaire de connexions OData](../integration-services/connection-manager/odata-connection-manager.md).

* La configuration d’Always Encrypted sur une table existante échoue avec des erreurs sur des objets non associés. [Microsoft Connect - ID 3103181](https://connect.microsoft.com/SQLServer/feedback/details/3103181/setting-up-always-encrypted-on-an-existing-table-fails-with-errors-on-unrelated-objects)

* La configuration d’Always Encrypted pour une base de données existante avec plusieurs schémas ne fonctionne pas. [Microsoft Connect - ID 3109591](https://connect.microsoft.com/SQLServer/feedback/details/3109591/sql-server-2016-always-encrypted-against-existing-database-with-multiple-schemas-doesnt-work)

* L’Assistant Colonne chiffrée d’Always Encrypted échoue parce que la base de données contient des vues qui référencent des vues système. [Microsoft Connect - ID 3111925](https://connect.microsoft.com/SQLServer/feedback/details/3111925/sql-server-2016-always-encrypted-encrypted-column-wizard-failed-task-failed-due-to-following-error-cannot-save-package-to-file-the-model-has-build-blocking-errors)

* Lors du chiffrement à l’aide d’Always Encrypted, les erreurs provenant de l’actualisation des modules après le chiffrement ne sont pas gérées correctement.

* Le menu *Ouvrir un fichier récent* n’affiche pas les fichiers récemment enregistrés. [Microsoft Connect - ID 3113288](https://connect.microsoft.com/SQLServer/feedback/details/3113288/ssms-2016-open-recent-menu-doesnt-show-recently-saved-files)

* SSMS est lent quand l’utilisateur clique avec le bouton droit sur un index pour une table (via une connexion (Internet) à distance). [Microsoft Connect - ID 3114074](https://connect.microsoft.com/SQLServer/feedback/details/3114074/ssms-slow-when-right-clicking-an-index-for-a-table-over-a-remote-internet-connection)
 
* Correction d’un problème avec la barre de défilement de SQL Designer. [Microsoft Connect - ID 3114856](http://connect.microsoft.com/SQLServer/feedback/details/3114856/bug-in-scrollbar-on-sql-desginer-in-ssms-2016)

* Le menu contextuel pour les tables se bloque momentanément 
 
* Il peut arriver que SSMS lève des exceptions dans le moniteur d’activité et se bloque. [Microsoft Connect - ID 697527](https://connect.microsoft.com/SQLServer/feedback/details/697527/)

* SSMS 2016 se bloque avec l’erreur « Le processus a été arrêté en raison d’une erreur interne dans le runtime .NET à l’adresse IP 71AF8579 (71AE0000) avec le code de sortie 80131506 »


## <a name="ssms-1651"></a>SSMS 16.5.1
Disponibilité générale | Numéro de build : 13.0.16100.1

* Correction d’un problème où Invoke-Sqlcmd insère par erreur plusieurs lignes lors d’une contrainte de validation. [Article Microsoft Connect n°811560](https://connect.microsoft.com/SQLServer/feedback/details/811560)

* Correction d’un problème où les versions en langue autre que le français ne fonctionnent pas complètement lors de la création de groupes de disponibilité.

* Correction d’un problème où un clic sur le fichier XML du plan de requête n’ouvre pas l’interface utilisateur SSMS appropriée.


## <a name="downloadssdtmediadownloadpng-ssms-165httpgomicrosoftcomfwlinklinkid832812"></a>![télécharger](../ssdt/media/download.png) [SSMS 16.5](http://go.microsoft.com/fwlink/?LinkID=832812)
Disponibilité générale | Numéro de build : 13.0.16000.28


[Chinois (République populaire de Chine)](https://go.microsoft.com/fwlink/?linkid=832812&clcid=0x804) | [Chinois (Taïwan)](https://go.microsoft.com/fwlink/?linkid=832812&clcid=0x404) | [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=832812&clcid=0x409) | [Français](https://go.microsoft.com/fwlink/?linkid=832812&clcid=0x40c) | [Allemand | ](https://go.microsoft.com/fwlink/?linkid=832812&clcid=0x407)[Italien](https://go.microsoft.com/fwlink/?linkid=832812&clcid=0x410) | [Japonais](https://go.microsoft.com/fwlink/?linkid=832812&clcid=0x411) | [Coréen](https://go.microsoft.com/fwlink/?linkid=832812&clcid=0x412) | [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=832812&clcid=0x416) | [Russe](https://go.microsoft.com/fwlink/?linkid=832812&clcid=0x419) | [Espagnol](https://go.microsoft.com/fwlink/?linkid=832812&clcid=0x40a)

* Résolution d’un problème : un blocage peut se produire quand une base de données avec un nom de la table contenant « ;: » a été activée.
* Résolution d’un problème : les modifications apportées à la page Modèle dans la fenêtre des propriétés de base de données tabulaire AS génèrent un script de la définition d’origine. 
[Élément Microsoft Connect : 3080744](https://connect.microsoft.com/SQLServer/feedback/details/3080744) 
* Correction du problème lié à l’ajout de fichiers temporaires à la liste « Fichiers récents ».  
[Élément Microsoft Connect : 2558789](https://connect.microsoft.com/SQLServer/feedback/details/2558789)
* Résolution du problème : l’élément de menu « Gérer la Compression » est désactivé pour les nœuds de la table utilisateur dans l’arborescence de l’Explorateur d’objets.  
[Élément Microsoft Connect : 3104616](https://connect.microsoft.com/SQLServer/feedback/details/3104616)

* Résolution du problème : l’utilisateur ne peut pas définir la taille de police pour l’Explorateur d’objets, l’Explorateur de serveurs inscrits, l’Explorateur de modèle ainsi que pour les détails de l’Explorateur d’objets. La police pour les explorateurs utilise la police de l’environnement.  
[Élément Microsoft Connect : 691432](https://connect.microsoft.com/SQLServer/feedback/details/691432)

* Résolution du problème : SSMS se reconnecte toujours à la base de données par défaut quand la connexion est perdue.  
[Élément Microsoft Connect Item : 3102337](https://connect.microsoft.com/SQLServer/feedback/details/3102337)

* Résolution d’un grand nombre de problèmes liés à la haute résolution dans la fenêtre de gestion des stratégies et de l’Éditeur de requête, notamment les icônes de plan d’exécution.

* Résolution du problème : l’option de configuration des polices et des couleurs pour les événements étendus est manquante.

* Résolution du problème : blocage de SSMS se produisant lors de la fermeture de l’application ou lors de la tentative d’affichage de la boîte de dialogue d’erreur.


## <a name="downloadssdtmediadownloadpng-ssms-1641-september-2016httpgomicrosoftcomfwlinklinkid828615"></a>![télécharger](../ssdt/media/download.png) [SSMS 16.4.1 (septembre 2016)](http://go.microsoft.com/fwlink/?LinkID=828615)
Disponibilité générale | Numéro de build : 13.0.15900.1

[Chinois (République populaire de Chine)](https://go.microsoft.com/fwlink/?linkid=828615&clcid=0x804) | [Chinois (Taïwan)](https://go.microsoft.com/fwlink/?linkid=828615&clcid=0x404) | [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=828615&clcid=0x409) | [Français](https://go.microsoft.com/fwlink/?linkid=828615&clcid=0x40c) | [Allemand | ](https://go.microsoft.com/fwlink/?linkid=828615&clcid=0x407)[Italien](https://go.microsoft.com/fwlink/?linkid=828615&clcid=0x410) | [Japonais](https://go.microsoft.com/fwlink/?linkid=828615&clcid=0x411) | [Coréen](https://go.microsoft.com/fwlink/?linkid=828615&clcid=0x412) | [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=828615&clcid=0x416) | [Russe](https://go.microsoft.com/fwlink/?linkid=828615&clcid=0x419) | [Espagnol](https://go.microsoft.com/fwlink/?linkid=828615&clcid=0x40a)

*  Correction d’un problème selon lequel la tentative de modification (ALTER) d’une procédure stockée échoue :  
[Article Microsoft Connect n°3103831](https://connect.microsoft.com/SQLServer/feedback/details/3103831)

* Nouvelles applets de commande « Read-SqlTableData », « Read-SqlViewData » et « Write-SqlTableData » pour afficher et écrire des données à l’aide de PowerShell.  
[Trello Read-SqlTableData Card](https://trello.com/c/FXVUNJ8x/131-read-sqltabledata)  
[Article Microsoft Connect n° 2685363](https://connect.microsoft.com/SQLServer/feedback/details/2685363)
    
* Nouvelle applet de commande « Add-SqlLogin » pour permettre de nouveaux scénarios de gestion de connexion à l’aide de PowerShell.  
[Article Microsoft Connect n° 2588952](https://connect.microsoft.com/SQLServer/feedback/details/2588952)
    
*  Prise en charge et utilisation améliorées pour les utilisateurs qui se connectent à plusieurs clouds nationaux.
    
    
*  Correction d’un problème où des exceptions Mémoire insuffisante étaient levées.  
[Article Microsoft Connect n° 3062914](https://connect.microsoft.com/SQLServer/feedback/details/3062914)  
[Article Microsoft Connect n° 3074856](https://connect.microsoft.com/SQLServer/feedback/details/3074856)
    
*  Correction d’un problème où le filtrage par schéma n’était pas une option de filtre valide.  
[Article Microsoft Connect n° 3058105](https://connect.microsoft.com/SQLServer/feedback/details/3058105)  
[Article Microsoft Connect n° 3101136](https://connect.microsoft.com/SQLServer/feedback/details/3101136)
    
*  Correction d’un problème où la fenêtre de surveillance d’une base de données étendue n’était pas accessible.
    
*  Correction d’un problème où la touche F1 (Aide) ouvrait toujours le contenu en ligne. Les utilisateurs peuvent désormais choisir s’ils préfèrent une aide en ligne ou hors connexion dans « Définir les préférences pour l’aide » du menu Aide.   
[Article Microsoft Connect n° 2826366](https://connect.microsoft.com/SQLServer/feedback/details/2826366)
    
*  Correction d’un problème où l’écriture du script d’un modèle tabulaire Analysis Services de niveau 1200 ne supprimait pas le mot de passe pour l’écriture du script, même si la version du serveur indiquait [l’objet de modèle de client est maintenant synchronisé avant l’écriture du script].
    
*  Correction d’un problème où l’option « SELECT TOP N ROWS » générait une syntaxe dépréciée pour l’opérateur TOP.  
[Article Microsoft Connect n° 3065435](https://connect.microsoft.com/SQLServer/feedback/details/3065435)
    
*  Correction de divers problèmes de mise en page dans SSMS, notamment la page Propriétés de la connexion et les options d’exécution de requête avancée.   
[Article Microsoft Connect n° 3058199](https://connect.microsoft.com/SQLServer/feedback/details/3058199)  
[Article Microsoft Connect n° 3079122](https://connect.microsoft.com/SQLServer/feedback/details/3058199)  
[Article Microsoft Connect n° 3071384](https://connect.microsoft.com/SQLServer/feedback/details/3071384)
    
*  Correction d’un problème où une solution était créée automatiquement chaque fois qu’un utilisateur ouvrait une nouvelle fenêtre de requête.   
[Article Microsoft Connect n° 2924667](https://connect.microsoft.com/SQLServer/feedback/details/2924667)    
[Article Microsoft Connect n° 2917742](https://connect.microsoft.com/SQLServer/feedback/details/2917742)   
[Article Microsoft Connect n° 2612635](https://connect.microsoft.com/SQLServer/feedback/details/2612635)
    
*  Correction d’un problème où les tables temporelles ne pouvaient pas être développées dans l’Explorateur d’objets quand elles se trouvaient dans des bases de données système.  
[Article Microsoft Connect n° 2551649](https://connect.microsoft.com/SQLServer/feedback/details/2551649)
    
*  Correction d’un problème où SSMS exécute une requête SELECT @@trancount après l’exécution d’un lot.    
[Article Microsoft Connect n° 3042364](https://connect.microsoft.com/SQLServer/feedback/details/3042364)
    
*  Correction d’un problème dans Analysis Services où la création d’un script à partir de la page de propriétés d’un serveur masquait une boîte de dialogue de connexion.
    
*  Correction d’un problème où Ctrl + Q ne sélectionnait pas la barre d’outils Lancement rapide.
    
*  Correction d’un problème où la modification du paramètre MaxSize d’une base de données à l’aide de la boîte de dialogue Propriétés du serveur ne fonctionnait pas pour les bases de données > 2 To.  
[Article Microsoft Connect n° 1231091](https://connect.microsoft.com/SQLServer/feedback/details/1231091)
    
*  Correction d’un problème où l’Assistant Restauration de la base de données n’acceptait pas les noms de fichiers avec des espaces de début :   
[Article Microsoft Connect n°2395147](https://connect.microsoft.com/SQLServer/feedback/details/2395147)



## <a name="downloadssdtmediadownloadpng-ssms-163-august-2016httpgomicrosoftcomfwlinklinkid824938"></a>![télécharger](../ssdt/media/download.png) [SSMS 16.3 (août 2016)](http://go.microsoft.com/fwlink/?LinkID=824938)
Généralement disponible | Numéro de version : 13.0.15700.28


[Chinois (République populaire de Chine)](https://go.microsoft.com/fwlink/?linkid=824938&clcid=0x804) | [Chinois (Taïwan)](https://go.microsoft.com/fwlink/?linkid=824938&clcid=0x404) | [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=824938&clcid=0x409) | [Français](https://go.microsoft.com/fwlink/?linkid=824938&clcid=0x40c) | [Allemand | ](https://go.microsoft.com/fwlink/?linkid=824938&clcid=0x407)[Italien](https://go.microsoft.com/fwlink/?linkid=824938&clcid=0x410) | [Japonais](https://go.microsoft.com/fwlink/?linkid=824938&clcid=0x411) | [Coréen](https://go.microsoft.com/fwlink/?linkid=824938&clcid=0x412) | [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=824938&clcid=0x416) | [Russe](https://go.microsoft.com/fwlink/?linkid=824938&clcid=0x419) | [Espagnol](https://go.microsoft.com/fwlink/?linkid=824938&clcid=0x40a)

* Les versions mensuelles de SSMS sont désormais étiquetées numériquement.

* [Nouvelle option d’authentification **« Authentification universelle Active Directory »**](https://azure.microsoft.com/documentation/articles/sql-database-ssms-mfa-authentication/). Il s’agit d’un système d’authentification par jeton piloté par Azure Active Directory, qui prend en charge des mécanismes d’authentification multifacteur, par mot de passe et intégrée.

* Nouveaux modèles d’événements étendus correspondant à la fonctionnalité des modèles SQL Server Profiler [(article Microsoft Connect n°2543925)](../tools/sql-server-profiler/sql-server-profiler-templates.md).

* Nouvelles boîtes de dialogue Créer une base de données et Propriétés de la base de données pour les bases de données SQL Azure.

* Nouvelles applets de commande « Get-SqlLogin » et « Remove-SqlLogin » pour effectuer la gestion des comptes de connexion SQL Server à l’aide de PowerShell.  
*Demandes de correctif connexes des clients :*   
[Article Microsoft Connect n°2588952.](https://connect.microsoft.com/SQLServer/feedback/details/2588952/)

* Nouvelle applet de commande PowerShell « New-SqlColumnMasterKeySettings » qui ajoute la prise en charge de la création de clés principales de colonne pour les fournisseurs arbitraires et les chemins d’accès de clé.

* Nouvelle boîte de dialogue « Créer une base de données » pour simplifier la création de bases de données SQL Azure dans SSMS>

* Prise en charge du filtrage dans le nœud « Bases de données » de l’Explorateur d’objets de SSMS. Accédez au nœud « Bases de données » dans l’Explorateur d’objets, puis cliquez sur l’icône de filtre dans la barre d’outils de l’Explorateur d’objet pour filtrer la liste des bases de données.

* Prise en charge des comptes de stockage de type Azure Resource Manager (ARM) dans les Assistants Sauvegarde et Restauration.

* [Prise en charge bêta initiale des écrans haute résolution](https://blogs.msdn.microsoft.com/sqlreleaseservices/ssms-highdpi-support/).  
*Demandes de correctif connexes des clients :*   
[Article Microsoft Connect n°1129301](https://connect.microsoft.com/SQLServer/feedback/details/1129301/management-studio-is-unusable-on-a-4k-display), [Article Microsoft Connect n°1858763](https://connect.microsoft.com/SQLServer/feedback/details/1858763/), [Article Microsoft Connect n°1852671](https://connect.microsoft.com/SQLServer/feedback/details/1852671/), [Article Microsoft Connect n°1487643](https://connect.microsoft.com/SQLServer/feedback/details/1487643/),  [Article Microsoft Connect n°1355641](https://connect.microsoft.com/SQLServer/feedback/details/1355641/), [Article Microsoft Connect n°2161595](https://connect.microsoft.com/SQLServer/feedback/details/2161595/), [Article Microsoft Connect n°1854041](https://connect.microsoft.com/SQLServer/feedback/details/1854041/), [Article Microsoft Connect n°1055617](https://connect.microsoft.com/SQLServer/feedback/details/1055617/), [Article Microsoft Connect n°2448774](https://connect.microsoft.com/SQLServer/feedback/details/2448774/), [Article Microsoft Connect n°1521405](https://connect.microsoft.com/SQLServer/feedback/details/1521405/), [Article Microsoft Connect n°2117853](https://connect.microsoft.com/SQLServer/feedback/details/2117853/), [Article Microsoft Connect n°2014256](https://connect.microsoft.com/SQLServer/feedback/details/2014256/), [Article Microsoft Connect n°2162218](https://connect.microsoft.com/SQLServer/feedback/details/2162218/), [Article Microsoft Connect n°2344551](https://connect.microsoft.com/SQLServer/feedback/details/2344551/), [Article Microsoft Connect n°1664436](https://connect.microsoft.com/SQLServer/feedback/details/1664436/), [Article Microsoft Connect n°2554043](https://connect.microsoft.com/SQLServer/feedback/details/2554043/), [Article Microsoft Connect n°2983216](https://connect.microsoft.com/SQLServer/feedback/details/2983216/), [Article Microsoft Connect n°2021706](https://connect.microsoft.com/SQLServer/feedback/details/2021706/)

* Améliorations de l’Assistant Paramétrage du moteur de base de données (DTA) pour prendre automatiquement en charge la lecture d’une charge de travail à partir du Magasin de requêtes de SQL Server.

* Améliorations de l’Assistant Paramétrage du moteur de base de données (DTA) pour afficher les recommandations d’index des index columnstore, columnstore non cluster et rowstore.

* Prise en charge de l’envoi de commandes DBCC (Database Console Commands) à l’aide d’applets de commande PowerShell de SQL Server Analysis Services.

* Résolution de bogue pour afficher le texte en clair de colonnes d’objet volumineux (LOB) AlwaysEncrypted déchiffrées dans SSMS.  
*Demandes de correctif connexes des clients :*   
[Article Microsoft Connect n°2413024](https://connect.microsoft.com/SQLServer/feedback/details/2413024/cannot-view-cleartext-of-alwaysencrypted-lob-columns-in-ssms)

* Résolution de bogue dans la boîte de dialogue Always Encrypted pour corriger l’incident qui se produit quand les styles visuels Windows ne sont pas activés (par exemple, l’activation de l’affichage à contraste élevé).

* Résolution de bogue pour l’erreur « Méthode introuvable » qui empêche la connexion à des instances SQL Server.

* Résolution de bogue pour l’incident SSMS qui se produit lors de la création d’une fonction de partition avec décalage de date/heure.

* Résolution de bogue pour ajouter l’exigence de disposer de Microsoft .NET 3.5 pour démarrer l’outil d’administration Distributed Replay (DReplay.exe).

* Résolution de bogue dans l’Assistant Déploiement d’Analysis Services pour prendre en charge les noms de serveur complets.

* Résolution de bogue dans SSMS pour afficher les partitions dans les modèles tabulaires Analysis Services avec un modèle de compatibilité 2016.  
*Demandes de correctif connexes des clients :*   
[Article Microsoft Connect n°2845053](https://connect.microsoft.com/SQLServer/feedback/details/2845053/ssms-cannot-display-partitions-in-tabular-models-in-2016-compatibility-level) 

* Améliorations des performances et résolutions de bogues dans les modèles tabulaires Analysis Services et les Objets de gestion partagée SQL Server. 


---
## <a name="downloadssdtmediadownloadpng-ssms-july-2016-hotfix-updatehttpgomicrosoftcomfwlinklinkid822301"></a>![télécharger](../ssdt/media/download.png) [mise à jour du correctif logiciel de SSMS, juillet 2016](http://go.microsoft.com/fwlink/?LinkID=822301)
Généralement disponible | Numéro de version : 13.0.15600.2

[Chinois (République populaire de Chine)](https://go.microsoft.com/fwlink/?linkid=822301&clcid=0x804) | [Chinois (Taïwan)](https://go.microsoft.com/fwlink/?linkid=822301&clcid=0x404) | [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=822301&clcid=0x409) | [Français](https://go.microsoft.com/fwlink/?linkid=822301&clcid=0x40c) | [Allemand | ](https://go.microsoft.com/fwlink/?linkid=822301&clcid=0x407)[Italien](https://go.microsoft.com/fwlink/?linkid=822301&clcid=0x410) | [Japonais](https://go.microsoft.com/fwlink/?linkid=822301&clcid=0x411) | [Coréen](https://go.microsoft.com/fwlink/?linkid=822301&clcid=0x412) | [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=822301&clcid=0x416) | [Russe](https://go.microsoft.com/fwlink/?linkid=822301&clcid=0x419) | [Espagnol](https://go.microsoft.com/fwlink/?linkid=822301&clcid=0x40a)

* **Résolution de bogue dans SSMS pour activer des options de menu contextuel manquantes**.  
*Demandes de correctif connexes des clients :*  
[Article Microsoft Connect n°2883440](https://connect.microsoft.com/SQLServer/feedback/details/2883440/lost-table-design-and-edit-top-n-rows-in-tables-context-menu)  
[Article Microsoft Connect n°2909644](https://connect.microsoft.com/SQLServer/feedback/details/2909644/ssms-2016-is-missing-edit-options-against-sql-express-2014)  
[Article Microsoft Connect n°2924345](https://connect.microsoft.com/SQLServer/feedback/details/2924345/some-ssms-object-explorer-right-click-menu-options-missing-in-july-update)

---
## <a name="ssms-july-2016-install-the-july-2016-hotfix"></a>SSMS, juillet 2016 (installer le correctif logiciel de juillet 2016)
Généralement disponible | Numéro de version : 13.0.15500.91

* *Modification, 5 juillet :* Amélioration de la prise en charge des bases de données tabulaires SQL Server 2016 (niveau de compatibilité 1200) dans la boîte de dialogue de processus Analysis Services et l’Assistant Déploiement d’Analysis Services.

* *Modification, 5 juillet :* Ajout d’une nouvelle option dans la boîte de dialogue des options d’exécution de requête de SSMS pour définir « XACT_ABORT ». Cette option est activée par défaut dans cette version de SSMS et demande à SQL Server de restaurer l’intégralité de la transaction et d’abandonner le traitement en cas d’erreur d’exécution.

* Prise en charge d’Azure SQL Data Warehouse dans SSMS.

* Mises à jour importantes apportées au module SQL Server PowerShell. Cela inclut un nouveau [module SQL PowerShell et de nouvelles applets de commande pour le chiffrement intégral, SQL Agent et les journaux d’erreurs SQL](https://blogs.technet.microsoft.com/dataplatforminsider/2016/06/30/sql-powershell-july-2016-update).

* Prise en charge de la génération de scripts PowerShell dans l’Assistant Chiffrement intégral.

* Délais de connexion aux bases de données SQL Azure considérablement améliorés.

* Nouvelle boîte de dialogue Backup to URL (Sauvegarde vers l’URL) pour prendre en charge la création d’informations d’identification de stockage Azure pour les sauvegardes de bases de données SQL Server 2016. Cette boîte de dialogue fournit une expérience plus fluide pour stocker les sauvegardes de bases de données dans un compte de stockage Azure.
 
* Nouvelle boîte de dialogue Restore (Restauration) pour simplifier la restauration d’une sauvegarde de base de données SQL Server 2016 à partir du service de stockage Microsoft Azure.
 
* Résolution de bogue dans le concepteur de requêtes SSMS pour autoriser l’ajout de tables au concepteur si un utilisateur ne dispose pas d’autorisations SELECT sur celles-ci.

* Résolution de bogue pour ajouter la prise en charge d’IntelliSense pour les fonctions « TRY_CAST() » et « TRY_CONVERT() ».  
*Demandes de correctif connexes des clients :*  
[Article Microsoft Connect n°2453461](https://connect.microsoft.com/SQLServer/feedback/details/2453461/sql-server-2012-issue-with-try-cast).

* Résolution de bogue dans le module PowerShell pour activer le chargement de l’extension Analysis Services « SQLAS ».  
*Demandes de correctif connexes des clients :*  
[Article Microsoft Connect n°2544902](https://connect.microsoft.com/SQLServer/feedback/details/2544902/ssms-march-2016-refresh-sqlps-failed-to-load-the-sqlas-extension).

* Résolution de bogue dans la fenêtre de l’éditeur de SSMS pour autoriser l’ouverture par glisser-déplacer de fichiers SQL.  
*Demandes de correctif connexes des clients :*  
[Article Microsoft Connect n°2690658](https://connect.microsoft.com/SQLServer/feedback/details/2690658/cannot-drag-sql-files-into-management-studios).

* Résolution de bogue dans Profiler pour résoudre les blocages de Profiler lors de la sortie.  
*Demandes de correctif connexes des clients :*  
[Article Microsoft Connect n°2616550](https://connect.microsoft.com/SQLServer/feedback/details/2616550/sql-server-2016-rc2-profiler-version-13-0-1300-275-wont-close-after-trace-is-started-even-after-trace-is-stopped).  
[Article Microsoft Connect n°2319968](https://connect.microsoft.com/SQLServer/Feedback/Details/2319968).

* Résolution de bogue dans SSMS pour éviter l’incident qui se produit lors d’une tentative de modification d’un lien de jointure dans le Concepteur de tables de SSMS.  
*Demandes de correctif connexes des clients :*  
[Article Microsoft Connect n°2721052](https://connect.microsoft.com/SQLServer/feedback/details/2721052/ssms-view-design-mode-right-click-on-join-crashes-ssms).

* Résolution de bogue dans SSMS pour activer la génération de script de base de données pour les membres du rôle db_owner.  
*Demandes de correctif connexes des clients :*  
[Article Microsoft Connect n°2869241](https://connect.microsoft.com/SQLServer/feedback/details/2869241/error-with-script-database-as-create-to-in-ssms-2008r2-and-ssms-2016-june).

* Résolution de bogue dans l’éditeur de SSMS pour supprimer le délai de fermeture d’un onglet de requête si le serveur a été mis hors connexion.  
*Demandes de correctif connexes des clients :*  
[Article Microsoft Connect n°2656058](https://connect.microsoft.com/SQLServer/feedback/details/2656058/ssms-2014-2016-query-tab-takes-significantly-longer-to-close-if-the-instance-it-was-connected-to-is-now-offline).

* Résolution de bogue pour activer l’option de sauvegarde dans les bases de données SQL Server Express. 
*Demandes de correctif connexes des clients :*  
[Article Microsoft Connect n°2801910](https://connect.microsoft.com/SQLServer/feedback/details/2801910/ssms-2016-backup-option-not-appearing-in-tasks).  
[Article Microsoft Connect n°2874434](https://connect.microsoft.com/SQLServer/feedback/details/2874434/backup-missing-from-tasks-context-menu-in-ssms-2016-when-you-are-connected-to-an-express-instance).

* Résolution de bogue dans Analysis Services pour afficher correctement le fournisseur de flux de données pour les modèles Analysis Services multidimensionnels.

----
## <a name="downloadssdtmediadownloadpng-ssms-june-2016httpgomicrosoftcomfwlinklinkid799832"></a>![télécharger](../ssdt/media/download.png) [SSMS, juin 2016](http://go.microsoft.com/fwlink/?LinkID=799832)
Généralement disponible | Numéro de version : 13.0.15000.23

[Chinois (République populaire de Chine)](https://go.microsoft.com/fwlink/?linkid=799832&clcid=0x804) | [Chinois (Taïwan)](https://go.microsoft.com/fwlink/?linkid=799832&clcid=0x404) | [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=799832&clcid=0x409) | [Français](https://go.microsoft.com/fwlink/?linkid=799832&clcid=0x40c) | [Allemand | ](https://go.microsoft.com/fwlink/?linkid=799832&clcid=0x407)[Italien](https://go.microsoft.com/fwlink/?linkid=799832&clcid=0x410) | [Japonais](https://go.microsoft.com/fwlink/?linkid=799832&clcid=0x411) | [Coréen](https://go.microsoft.com/fwlink/?linkid=799832&clcid=0x412) | [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=799832&clcid=0x416) | [Russe](https://go.microsoft.com/fwlink/?linkid=799832&clcid=0x419) | [Espagnol](https://go.microsoft.com/fwlink/?linkid=799832&clcid=0x40a)

* SSMS est généralement disponible depuis la version de juin 2016.

* La nouvelle boîte de dialogue Recherche rapide dans SSMS est mieux intégrée dans le document actif, et permet de rechercher des expressions régulières. 
*Demandes de correctif connexes des clients :*  
<https://connect.microsoft.com/SQLServer/feedback/details/2735513/quick-find-replace-in-ssms-2016-rc3/>

* Améliorations du programme d’installation de SSMS permettant de suivre la progression de l’installation et les codes de sortie du processus pour les installations sans assistance à l’aide de scripts.

* Résolution de bogue dans l’aide contextuelle (accessible via la touche F1) de SSMS afin d’afficher correctement les documents et articles d’aide.

* Résolution de bogue dans la vue « Requêtes régressées » du Magasin de données de requête, qui entraînait le blocage de SSMS lors du défilement.

* Résolution de bogue dans le connecteur Analysis Services OLEDB d’Excel pour autoriser les connexions d’Excel 2016 à SQL Server Analysis Services.

* Résolution de bogue dans la boîte de dialogue Connexion de SSMS pour afficher la boîte de dialogue de connexion sur le même moniteur que la fenêtre principale de SSMS dans les systèmes multimoniteur.  
*Demandes de correctif connexes des clients :*  
<https://connect.microsoft.com/SQLServer/feedback/details/724909/connection-dialog-appears-off-screen/>
<https://connect.microsoft.com/SQLServer/feedback/details/755689/sql-server-management-studio-connect-to-server-popup-dialog/>  
<https://connect.microsoft.com/SQLServer/feedback/details/389165/sql-server-management-studio-gets-confused-dealing-with-multiple-displays/>

* Résolution de bogue dans l’expérience Always Encrypted. Résolution du bogue où l’option de menu Always Encrypted n’était pas activée correctement pour les bases de données Stretch. Également résolution du bogue de l’Assistant Always Encrypted qui n’utilisait pas correctement le fournisseur HSM SafeNet (Luna SA).


## <a name="additional-downloads"></a>Téléchargements supplémentaires  
Pour obtenir la liste de tous les téléchargements de SQL Server Management Studio, rechercher dans le [Centre de téléchargement Microsoft](https://www.microsoft.com/en-us/download/search.aspx?q=sql%20server%20management%20studio&p=0&r=10&t=&s=Relevancy~Descending).  
  
Pour obtenir la dernière version de SQL Server Management Studio, voir [Télécharger SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md).  
