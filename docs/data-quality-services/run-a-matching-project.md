---
title: Exécuter un projet de correspondance | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: data-quality-services
ms.component: data-quality-services
ms.reviewer: ''
ms.suite: sql
ms.technology:
- data-quality-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.dqs.matchingproject.map.f1
- sql13.dqs.matchingproject.matching.f1
- sql13.dqs.matchingproject.export.f1
ms.assetid: 6aa9d199-83ce-4b5d-8497-71eef9258745
caps.latest.revision: 36
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 64d7866dac9b4886f12f215c5aa366960e25a03b
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="run-a-matching-project"></a>Exécuter un projet de correspondance

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  Cette rubrique décrit comment réaliser la correspondance de données dans [!INCLUDE[ssDQSnoversion](../includes/ssdqsnoversion-md.md)] (DQS). Le processus de correspondance identifie les clusters des enregistrements correspondants selon les règles de correspondance de la stratégie de correspondance, indique un enregistrement de chaque cluster en tant que survivant basé sur une règle de survivance, et exporte les résultats. DQS exécute le processus de correspondance, également appelé déduplication, dans un processus assisté par ordinateur, mais vous créez des règles de correspondance de manière interactive et sélectionnez la règle de survivance parmi plusieurs choix, afin de contrôler le processus de correspondance.  
  
 La correspondance est effectuée en trois étapes : un processus de mappage dans lequel vous identifiez la source de données et mappez les domaines à la source de données, un processus de correspondance dans lequel vous exécutez l'analyse de correspondance, et un processus de survivance et d'exportation dans lequel vous indiquez la règle de survivance et exportez les résultats de correspondance. Chacun de ces processus est exécuté sur une page distincte de l'Assistant de l'activité de correspondance, ce qui vous permet de vous déplacer dans les deux sens vers différentes pages, de réexécuter le processus et de fermer un processus de concordance spécifique, puis de retourner à la même étape du processus. DQS vous fournit des statistiques sur les données sources, les règles de correspondance et les résultats de correspondance qui vous permettent de prendre des décisions avisées sur la correspondance et d'affiner le processus de concordance.  
  
 Vous devez vous préparer à la correspondance en créant une stratégie de correspondance avec une ou plusieurs règles de correspondance, et en exécutant la stratégie sur des exemples de données. Le processus de projet de correspondance est distinct du processus de stratégie de correspondance, et une base de connaissances n'est pas remplie avec les connaissances de correspondance acquises à partir du projet de correspondance. Pour plus d'informations sur la création d'une stratégie de correspondance, consultez [Create a Matching Policy](../data-quality-services/create-a-matching-policy.md).  
  
##  <a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="Prerequisites"></a> Conditions préalables  
  
-   Vous devez avoir créé une base de connaissances avec une stratégie de correspondance composée d'une ou de plusieurs règles de correspondance.  
  
-   Microsoft Excel doit être installé sur l'ordinateur [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)] si les données sources à mettre en correspondance sont dans un fichier Excel. Sinon, vous ne pourrez pas sélectionner le fichier Excel à l'étape de mappage. Les fichiers créés par Microsoft Excel peuvent avoir une extension .xlsx, .xls ou .csv. Si la version 64 bits d'Excel est utilisée, seuls les fichiers Excel 2003 (.xls) sont pris en charge ; les fichiers Excel 2007 ou 2010 (.xlsx) ne sont pas pris en charge. Si vous utilisez la version 64 bits d'Excel 2007 ou 2010, enregistrez le fichier comme fichier .xls ou fichier .csv, ou installez une version 32 bits d'Excel à la place.  
  
###  <a name="Security"></a> Sécurité  
  
####  <a name="Permissions"></a> Permissions  
 Vous devez disposer du rôle dqs_kb_editor ou dqs_administrator sur la base de données DQS_MAIN pour exécuter un projet de correspondance.  
  
##  <a name="StartingaMatchingProject"></a> Première étape : Démarrage d'un projet de correspondance  
 Vous effectuez l'activité de correspondance dans un projet de qualité des données que vous créez dans l'application cliente DQS.  
  
1.  [!INCLUDE[ssDQSInitialStep](../includes/ssdqsinitialstep-md.md)] [Exécutez l’application Data Quality Client](../data-quality-services/run-the-data-quality-client-application.md).  
  
2.  Dans l'écran d'accueil de [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)] , cliquez sur **Nouveau projet de qualité des données** pour effectuer une correspondance dans un nouveau projet de qualité des données. Entrez un nom pour le projet de qualité des données, entrez une description, puis sélectionnez la base de connaissances que vous souhaitez utiliser pour la correspondance dans **Utiliser la base de connaissances**. Cliquez sur **Correspondance** pour l'activité. Cliquez sur **Suivant** pour passer à l'étape de mappage.  
  
3.  Cliquez sur **Ouvrir le projet de qualité des données** pour effectuer une correspondance dans un projet de qualité des données existant. Sélectionnez le projet, puis cliquez sur **Suivant**. (Vous pouvez aussi cliquer sur un projet sous **Projet de qualité des données récent**.) Si vous ouvrez un projet de correspondance qui est fermé, vous passez à l’étape où l’activité du projet de correspondance avait été fermée (comme indiqué par la colonne **État** dans la table de projet ou dans le nom du projet sous **Projet de qualité des données récent**). Si vous ouvrez un projet de correspondance qui est terminé, vous passez à la page **Exporter** (et vous ne pouvez pas revenir aux écrans précédents).  
  
##  <a name="MappingStage"></a> Étape de mappage  
 Dans l'étape de mappage, vous identifiez la source des données sur lesquelles vous allez exécuter l'analyse de correspondance et vous mappez les colonnes sources aux domaines pour rendre les domaines disponibles pour l'activité de correspondance.  
  
1.  Dans la page **Mapper** , pour effectuer la correspondance sur une base de données, laissez **Source de données** comme étant **SQL Server**, sélectionnez la base de données sur laquelle vous souhaitez effectuer la correspondance, puis sélectionnez la table. La base de données source doit être présente dans la même instance SQL Server que le serveur DQS. Sinon, elle ne s'affichera pas dans la liste déroulante.  
  
2.  Pour exécuter la correspondance sur les données dans une feuille de calcul Excel, sélectionnez **Fichier Excel** pour **Source de données**, cliquez sur **Parcourir** et sélectionnez le fichier Excel, et laissez **Utiliser la première ligne comme en-tête** sélectionné le cas échéant. Dans **Feuille de calcul**, sélectionnez la feuille de calcul du fichier Excel qui sera la source des données. Excel doit être installé sur l'ordinateur [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)] pour sélectionner un fichier Excel. Si Excel n'est pas installé sur l'ordinateur [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)] , le bouton **Parcourir** n'est pas disponible, et vous êtes averti sous cette zone de texte qu'Excel n'est pas installé.  
  
3.  Sous **Mappages**, sélectionnez un champ de la source de données pour **Colonne source**, puis sélectionnez le domaine correspondant. Répétez ces étapes pour tous les domaines que vous utilisez dans le processus de correspondance. Chaque domaine qui est défini dans la stratégie de correspondance doit être mappé à la colonne source appropriée. La page Mapper affiche les domaines définis dans la stratégie de correspondance et les règles dans la stratégie de correspondance dans le volet droit.  
  
    > [!NOTE]  
    >  Vous pouvez mapper vos données source à un domaine DQS uniquement si le type de données source est pris en charge dans DQS et correspond au type de données du domaine DQS. Pour plus d'informations sur les types de données pris en charge dans DQS, consultez [Types de données SQL Server et SSIS pris en charge pour les domaines DQS](../data-quality-services/supported-sql-server-and-ssis-data-types-for-dqs-domains.md).  
  
4.  Cliquez sur le contrôle **plus (+)** pour ajouter une ligne à la table Mappages ou sur le contrôle **moins (–)** pour en supprimer une.  
  
5.  Cliquez sur **Aperçu de la source de données** pour afficher les données de la table ou de la vue SQL Server que vous avez sélectionnée, ou de la feuille de calcul Excel que vous avez sélectionnée.  
  
6.  Cliquez sur **Afficher/Sélectionner des domaines composites** pour afficher la liste des domaines composites disponibles dans la base de connaissances et pour effectuer les sélections appropriées pour le mappage.  
  
7.  Cliquez sur **Suivant** pour passer à l'étape de correspondance.  
  
    > [!NOTE]  
    >  Cliquez sur **Fermer** pour enregistrer l'étape du projet de correspondance et revenir à la page d'accueil de DQS. La prochaine fois que vous ouvrirez ce projet, il démarrera à partir de la même étape. Cliquez sur **Annuler** pour mettre fin à l'activité de correspondance, auquel cas vous perdrez votre travail, et revenir à la page d'accueil de DQS.  
  
##  <a name="MatchingStage"></a> Étape de correspondance  
 Dans cette étape, vous exécutez un processus de correspondance assisté par ordinateur qui vous indique le nombre de correspondances trouvées dans les données sources selon les règles de correspondance. Ce processus génère une table de résultats de correspondance qui affiche les clusters que DQS a identifiés, chaque enregistrement dans le cluster avec son ID d'enregistrement et son score de correspondance, et l'enregistrement de début initial pour le cluster. L'enregistrement de début du cluster est sélectionné de manière aléatoire. Vous déterminez l'enregistrement survivant en sélectionnant la règle de survivance à la page **Exporter** lorsque vous exécutez le projet de correspondance. Chaque ligne supplémentaire dans un cluster est considérée comme une correspondance ; son score de correspondance (comparé à l'enregistrement de début) est fourni dans la table de résultats. Le numéro de cluster est identique à l'ID d'enregistrement pour l'enregistrement de début dans le cluster.  
  
 Dans les résultats de correspondance, vous pouvez filtrer sur les données souhaitées et rejeter les correspondances dont vous ne voulez pas. Vous pouvez afficher des données de profilage pour le processus de correspondance dans son ensemble, des détails sur les règles de correspondance qui sont appliquées et des statistiques sur les résultats de correspondance en général. Le processus de correspondance peut identifier les clusters avec ou sans chevauchement et, s'il est exécuté plusieurs fois, peut être exécuté sur des données récemment copiées de la source et réindexées, ou sur les anciennes données.  
  
1.  Sur la page **Correspondance**, sélectionnez **Clusters qui se chevauchent** dans la liste déroulante pour afficher les enregistrements pivots et les enregistrements suivants pour tous les clusters lorsque la correspondance est exécutée, même si les groupes de clusters ont des enregistrements en commun. Sélectionnez **Clusters qui ne se chevauchent pas** pour afficher les clusters qui ont des enregistrements en commun comme un seul cluster lorsque la correspondance est exécutée.  
  
2.  Cliquez sur **Recharger les données à partir de la source** (valeur par défaut) pour copier les données de la source de données dans la table intermédiaire et les réindexer lorsque vous exécutez le projet de correspondance. Cliquez sur **Exécuter sur les données précédentes** pour exécuter un projet de correspondance sans copier les données dans la table intermédiaire et les réindexer. **Exécuter sur les données précédentes** est désactivé pour la première exécution du projet de correspondance ou si vous modifiez le mappage dans la page **Mapper** et appuyez sur **Oui** dans le message suivant. Dans ces deux cas, vous devez effectuer une réindexation. Il n'est pas nécessaire de réindexer si le projet de correspondance n'a pas changé. L'exécution sur les données précédentes peut améliorer les performances.  
  
3.  Cliquez sur **Démarrer** pour exécuter la correspondance sur la source de données sélectionnée.  
  
4.  Cliquez sur **Arrêter** si vous souhaitez arrêter le projet de correspondance et ignorer les résultats.  
  
5.  Une fois le processus de correspondance terminé, vérifiez que les clusters dans la table **Résultats de correspondance** sont appropriés et affichez les statistiques sous les onglets **Générateur de profils** et **Résultats de correspondance** pour vérifier que vous obtenez les résultats dont vous avez besoin. Affichez les enregistrements correspondants en sélectionnant **Correspondants** pour **Filtre** ou affichez les enregistrements sans correspondance en sélectionnant **Sans correspondance**.  
  
6.  Si vous avez plusieurs règles de correspondance dans la stratégie de correspondance, cliquez sur l'onglet **Règles de correspondance** pour identifier l'icône de chaque règle, puis vérifiez la règle qui a identifié un enregistrement comme correspondance en identifiant la règle dans la colonne **Règle** de la table **Résultats de correspondance** .  
  
7.  Si vous sélectionnez un enregistrement non-pivot dans la table et cliquez sur l'icône **Afficher les détails** (ou double-cliquez sur l'enregistrement), DQS affiche un message **Détails du score de correspondance** qui affiche l'enregistrement sur lequel vous avez double-cliqué et son enregistrement pivot (et les valeurs de tous leurs champs), le score entre eux, et une exploration des contributions au score de correspondance de chaque champ. Un double-clic sur un enregistrement pivot n'affiche pas le message.  
  
8.  Cliquez sur l'icône **Réduire tout** pour réduire les enregistrements affichés dans la table **Résultats de correspondance** afin d'inclure uniquement l'enregistrement pivot, et non les enregistrements en double. Cliquez sur **Développer tout** pour développer les enregistrements affichés dans la table Résultats de correspondance afin d'inclure tous les enregistrements en double.  
  
9. Pour rejeter un enregistrement des résultats de correspondance, activez la case à cocher **Rejeté** de l'enregistrement.  
  
10. Pour changer le score de correspondance minimal qui détermine le niveau de correspondance qu’un enregistrement doit avoir pour s’afficher, sélectionnez l’icône **Score de correspondance minimal** au-dessus du côté droit de la table et entrez un nombre supérieur. Le score de correspondance minimal a la valeur 80 % par défaut. Cliquez sur **Actualiser** pour modifier le contenu de la table.  
  
11. Une fois l'analyse terminée, le bouton **Démarrer** se transforme en bouton **Redémarrer** . Cliquez sur **Redémarrer** pour exécuter à nouveau le projet d'analyse. Toutefois, les résultats de l'analyse précédente n'ayant pas encore été enregistrés, le fait de cliquer sur **Redémarrer** entraînera la perte de ces données précédentes. Pour continuer, cliquez sur **Oui** dans le message. Pendant l'exécution de l'analyse, ne quittez pas la page, car le processus d'analyse s'arrête alors.  
  
12. Cliquez sur **Suivant** pour passer à l'étape de survivance et d'exportation.  
  
##  <a name="SurvivorshipandExportStage"></a> Étape de survivance et d'exportation  
 Dans le processus de survivance, Data Quality Services détermine un enregistrement survivant pour chaque cluster, qui remplace les autres enregistrements correspondant dans le cluster. Il exporte ensuite les résultats de correspondance et/ou de survivance vers une table dans la base de données SQL Server, un fichier .csv ou un fichier Excel.  
  
 La survivance est facultative. Vous pouvez exporter les résultats sans exécuter la survivance, auquel cas DQS utilise l'enregistrement pivot qui a été indiqué dans l'analyse de correspondance. Si plusieurs enregistrements dans un cluster sont conformes à la règle de survivance, le processus de survivance sélectionne l'ID d'enregistrement le plus bas parmi les enregistrements en conflit pour être le survivant. Vous pouvez exporter des survivants vers divers fichiers ou tables en utilisant différentes règles de survivance.  
  
1.  Dans la page **Exporter** , sélectionnez la destination de l'exportation des données de correspondance dans **Type de destination**: **SQL Server**, **Fichier CSV**ou **Fichier Excel**.  
  
    > [!IMPORTANT]  
    >  Si vous utilisez une version 64 bits d'Excel, vous ne pouvez pas exporter les données correspondantes vers un fichier Excel : vous ne pouvez exporter que vers une base de données SQL Server ou un fichier .csv.  
  
2.  Si vous avez sélectionné **SQL Server** pour **Type de destination**, sélectionnez la base de données dans laquelle exporter les résultats dans **Nom de la base de données**.  
  
    > [!IMPORTANT]  
    >  La base de données de destination doit être présente dans la même instance de SQL Server que le serveur DQS. Sinon, elle ne s'affichera pas dans la liste déroulante.  
  
3.  Activez la case à cocher pour **Résultats de correspondance** pour exporter les résultats de correspondance (voir ci-dessus pour une explication) vers la table désignée dans une base de données SQL Server ou dans le fichier .csv ou Excel indiqué. Activez la case à cocher pour **Résultats de la survivance** pour exporter les résultats de survivance (voir ci-dessus pour une explication) vers la table désignée dans une base de données SQL Server ou dans le fichier .csv ou Excel indiqué.  
  
     Les éléments suivants sont exportés pour les résultats de correspondance :  
  
    -   Une liste de clusters et les enregistrements correspondants dans chaque cluster, y compris le nom de la règle et le score. L'enregistrement pivot est marqué comme « Pivot ». Les clusters s'affichent en premier dans la liste d'exportation.  
  
    -   Une liste des enregistrements sans correspondance, avec « NULL » dans les colonnes de score et de nom de la règle. Ces enregistrements sont ajoutés à la liste d'exportation après les clusters.  
  
     Les éléments suivants sont exportés pour les résultats de survivance :  
  
    -   Une liste des enregistrements survivants comme déterminée par le processus de survivance selon la règle de survivance. Ces enregistrements s'affichent en premier dans la liste d'exportation.  
  
    -   Une liste des enregistrements sans correspondance qui ne sont pas inclus dans les clusters des enregistrements correspondants. Ces enregistrements sont ajoutés après les résultats de survivance.  
  
4.  Si vous avez sélectionné **SQL Server** pour **Type de destination**, entrez le nom des tables dans lesquelles exporter les résultats dans **Nom de la table**. Si vous exportez à la fois les résultats de correspondance et les résultats de survivance, les tables de destination doivent avoir des noms différents qui sont propres à la base de données.  
  
5.  Si vous avez sélectionné **Fichier CSV** pour **Type de destination**, entrez le fichier et le chemin d'accès du fichier CSV dans lequel exporter dans **Nom du fichier CSV**.  
  
6.  Si vous avez sélectionné **Fichier Excel** pour **Type de destination**, entrez le fichier et le chemin d'accès du fichier Excel dans lequel exporter dans **Nom du fichier Excel**. Vous ne pouvez pas exporter vers un fichier Excel si vous utilisez une version 64 bits d'Excel.  
  
7.  Sélectionnez la règle de survivance comme suit :  
  
    -   Sélectionnez **Enregistrement pivot** (valeur par défaut) pour identifier l'enregistrement survivant en tant qu'enregistrement pivot initial choisi arbitrairement par DQS.  
  
    -   Sélectionnez **Enregistrement le plus complet et le plus long** pour identifier l'enregistrement survivant comme celui avec le plus grand nombre de champs remplis et qui a le plus grand nombre de termes dans chaque champ. Tous les champs sources sont vérifiés, même ceux qui n'ont pas été mappés à un domaine dans la page **Mapper** .  
  
    -   Sélectionnez **Enregistrement le plus complet** pour identifier l'enregistrement survivant comme celui avec le plus grand nombre de champs remplis. Un champ rempli contient au moins une valeur (chaîne, numérique, ou les deux). Tous les champs sources sont vérifiés, même ceux qui n'ont pas été mappés à un domaine dans la page Mapper. Un champ rempli contient au moins une valeur (chaîne, numérique, ou les deux).  
  
    -   Sélectionnez **Enregistrement le plus long** pour identifier l'enregistrement survivant comme celui avec le plus grand nombre de termes dans ses champs sources. Pour déterminer la longueur de chaque enregistrement, DQS vérifie la longueur des termes dans tous les champs sources, même les champs qui n'ont pas été mappés à un domaine dans la page **Mapper** .  
  
8.  Affichez les statistiques sous l'onglet **Générateur de profils** pour vérifier que vous obtenez les résultats dont vous avez besoin.  
  
9. Cliquez sur **Exporter** pour exporter les résultats. Cela affiche une boîte de dialogue Exportation de correspondance qui indique la progression, puis les résultats de l'exportation.  
  
    -   Si vous avez sélectionné **SQL Server** comme destination des données, une nouvelle table portant le nom spécifié sera créée dans la base de données sélectionnée.  
  
    -   Si vous avez sélectionné **Fichier CSV** comme destination des données, un fichier .csv sera créé à l'emplacement sur l'ordinateur [!INCLUDE[ssDQSServer](../includes/ssdqsserver-md.md)] avec le nom de fichier que vous avez spécifié précédemment dans la zone **Nom du fichier Csv** .  
  
    -   Si vous avez sélectionné **Fichier Excel** comme destination des données, un fichier .xlsx sera créé à l'emplacement sur l'ordinateur [!INCLUDE[ssDQSServer](../includes/ssdqsserver-md.md)] avec le nom de fichier que vous avez spécifié précédemment dans la zone **Nom du fichier Excel** .  
  
10. Vérifiez que l'exportation s'est terminée avec succès, puis cliquez sur **Fermer**.  
  
11. Cliquez sur **Terminer** pour terminer le projet de correspondance.  
  
    > [!NOTE]  
    >  Si vous avez terminé un projet de correspondance et l'utilisez de nouveau, il utilise la base de connaissances en place lorsqu'il a été publié. Il n'utilise aucune modification que vous avez apportée à la base de connaissances depuis que vous avez terminé le projet. Pour utiliser ces modifications, ou une nouvelle base de connaissances, vous devez créer un projet de correspondance. En revanche, si vous avez créé, mais pas fini, un projet de correspondance, toutes les modifications que vous avez publiées dans la stratégie de correspondance sont utilisées si vous exécutez une correspondance dans le projet.  
  
##  <a name="FollowUp"></a> Suivi : Après l'exécution d'un projet de correspondance  
 Après avoir exécuté un projet de correspondance, vous pouvez modifier la stratégie de correspondance dans la base de connaissances, puis créer et exécuter un autre projet de correspondance basé sur la stratégie de correspondance mise à jour. Pour plus d’informations, consultez [Create a Matching Policy](../data-quality-services/create-a-matching-policy.md).  
  
##  <a name="Profiler"></a> Onglets Générateur de profils et Résultats  
 Les onglets Générateur de profils et Résultats contiennent des statistiques pour le processus de correspondance.  
  
### <a name="profiler-tab"></a>Onglet Générateur de profils  
 Cliquez sur l'onglet **Générateur de profils** pour afficher des statistiques pour la base de données source et pour chaque champ inclus dans la règle de stratégie. Les statistiques sont mises à jour lorsque la règle de stratégie est exécutée. Le profilage vous aidera à évaluer l'efficacité du processus de déduplication ainsi qu'à déterminer jusqu'à quel point le processus peut améliorer la qualité des données. La précision dans le profilage n'est pas importante pour un projet de correspondance.  
  
 Les statistiques de la base de données source sont les suivantes :  
  
-   **Enregistrements**: nombre total d'enregistrements dans la base de données  
  
-   **Valeurs totales**: nombre total de valeurs dans les champs  
  
-   **Nouvelles valeurs**: nombre total de valeurs qui sont nouvelles depuis l'exécution précédente, et leur pourcentage par rapport à la totalité  
  
-   **Valeurs uniques**: nombre total de valeurs uniques dans les champs, et leur pourcentage par rapport à la totalité  
  
-   **Nouvelles valeurs uniques**: nombre total de valeurs uniques qui sont nouvelles dans les champs, et leur pourcentage par rapport à la totalité  
  
 Les statistiques de champ sont les suivantes :  
  
-   **Champ**: nom du champ qui a été inclus dans les mappages.  
  
-   **Domaine**: nom du domaine qui a été mappé au champ.  
  
-   **Nouveau**: nombre de nouvelles correspondances trouvées et leur pourcentage par rapport au total  
  
-   **Unique**: nombre d'enregistrements uniques dans le champ et leur pourcentage par rapport au total  
  
-   **Exhaustivité**: pourcentage d'achèvement de l'exécution de la règle.  
  
### <a name="matching-policy-notifications"></a>Notifications de stratégie de correspondance  
 Pour l'activité de stratégie de correspondance, les conditions suivantes génèrent des notifications :  
  
-   Le champ est vide dans tous les enregistrements ; il est recommandé de l'éliminer du mappage.  
  
-   Le score d'achèvement du champ est très faible ; vous pouvez l'éliminer du mappage.  
  
-   Toutes les valeurs d'un champ sont non valides ; vous devez vérifier le mappage et la pertinence des règles de domaine par rapport au contenu du champ.  
  
-   Il existe un faible niveau de valeurs valides dans le champ ; vous devez vérifier le mappage et la pertinence des règles de domaine par rapport au contenu du champ.  
  
-   Il existe un niveau élevé d'unicité dans ce champ. L'utilisation de ce champ dans la stratégie de correspondance peut diminuer les résultats de correspondance.  
  
### <a name="matching-rules-tab"></a>Onglet Règles de correspondance  
 Cliquez sur cet onglet pour afficher une liste des règles dans la stratégie de correspondance et les conditions dans une règle.  
  
 **Liste des règles**  
 Affiche une liste de toutes les règles de correspondance dans la stratégie de correspondance. Sélectionnez l'une des règles pour afficher les conditions de la règle dans la table Règle de correspondance.  
  
 **Table Règle de correspondance**  
 Affiche chaque condition dans la règle sélectionnée, notamment le domaine, la valeur de similarité, le poids et la sélection des éléments requis.  
  
### <a name="matching-results-tab"></a>Onglet Résultats de correspondance  
 Cliquez sur l'onglet **Résultats de correspondance** pour afficher des statistiques pour l'analyse de la source de données à l'aide de la connaissance sélectionnée pour le projet et des règles de correspondance dans cette base de connaissances. Les statistiques sont les suivantes :  
  
-   Nombre total d'enregistrements dans la base de données  
  
-   Nombre total d'enregistrements correspondants dans la base de données  
  
-   Nombre d'enregistrements dans la base de données qui ne sont pas considérés comme des doublons  
  
-   Nombre de clusters découverts  
  
-   Taille de cluster moyenne (nombre d'enregistrements en double divisé par le nombre de clusters)  
  
-   Nombre le moins élevé de doublons dans un cluster  
  
-   Nombre le plus élevé de doublons dans un cluster  
  
  
