---
title: Créer une stratégie de correspondance | Microsoft Docs
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
- sql13.dqs.kb.kbmatchingmap.f1
- sql13.dqs.kb.kbmatchingpolicy.f1
- sql13.dqs.kb.kbmatchingresults.f1
ms.assetid: cce77a06-ca31-47b6-8146-22edf001d605
caps.latest.revision: 43
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 7abc199b280d4e7da2757631b529395035542237
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-a-matching-policy"></a>Créer une stratégie de correspondance

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  Cette rubrique explique comment créer une stratégie de correspondance dans une base de connaissances dans [!INCLUDE[ssDQSnoversion](../includes/ssdqsnoversion-md.md)] (DQS). Vous vous préparez au processus de correspondance dans DQS en exécutant l'activité Stratégie de correspondance sur des exemples de données. Dans cette activité, vous créez et testez une ou plusieurs règles de correspondance dans la stratégie, puis vous publiez la base de connaissances afin que les règles de correspondance soient publiquement disponibles. Il ne peut y avoir qu'une seule stratégie de correspondance dans une base de connaissances, mais cette stratégie peut contenir plusieurs règles de correspondance.  
  
 La création d'une stratégie de correspondance s'effectue en trois étapes : un processus de mappage au cours duquel vous identifiez la source de données et mappez des domaines à des colonnes, un processus de stratégie de correspondance au cours duquel vous créez une ou plusieurs règles de correspondance et testez chaque règle de correspondance séparément, et un processus de résultats de correspondance au cours duquel vous exécutez toutes les règles de correspondance ensemble, et si vous en êtes satisfait, vous ajoutez la stratégie à la base de connaissances. Chacun de ces processus est exécuté sur une page distincte de l'Assistant de l'activité Correspondance, ce qui vous permet de naviguer entre les différentes pages, de réexécuter le processus et de fermer un processus de stratégie de correspondance spécifique, puis de retourner à la même étape du processus. Après avoir testé toutes les règles ensemble, si vous le souhaitez, vous pouvez retourner à la page **Stratégie de correspondance** , apporter quelques ajustements à une règle individuelle, la tester de nouveau séparément, puis retourner à la page **Résultats de correspondance** pour réexécuter toutes les règles ensemble. DQS vous fournit des statistiques sur les données sources, les règles de correspondance et les résultats de correspondance, lesquelles vous permettent de prendre des décisions avisées sur la stratégie de correspondance et de l'affiner.  
  
##  <a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="Prerequisites"></a> Conditions préalables  
 Microsoft Excel doit être installé sur l'ordinateur [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)] si les données sources se trouvent dans un fichier Excel. Sinon, vous ne pourrez pas sélectionner le fichier Excel à l'étape de mappage. Les fichiers créés par Microsoft Excel peuvent avoir une extension .xlsx, .xls ou .csv. Si la version 64 bits d'Excel est utilisée, seuls les fichiers Excel 2003 (.xls) sont pris en charge ; les fichiers Excel 2007 ou 2010 (.xlsx) ne sont pas pris en charge. Si vous utilisez la version 64 bits d'Excel 2007 ou 2010, enregistrez le fichier comme fichier .xls ou fichier .csv, ou installez une version 32 bits d'Excel à la place.  
  
###  <a name="Security"></a> Sécurité  
  
####  <a name="Permissions"></a> Permissions  
 Vous devez disposer du rôle dqs_kb_editor ou dqs_administrator sur la base de données DQS_MAIN pour créer une stratégie de correspondance.  
  
##  <a name="MatchingRules"></a> Comment définir des paramètres de règle de correspondance  
 La création d'une règle de correspondance est un processus itératif au cours duquel vous entrez les facteurs utilisés pour déterminer si un enregistrement est une correspondance d'un autre enregistrement. Vous pouvez entrer des conditions pour tout domaine dans une table. Lorsque DQS effectue une correspondance sur deux enregistrements, il compare les valeurs des champs mappés aux domaines qui sont inclus dans la règle de correspondance. DQS analyse les valeurs contenues dans chaque champ de la règle, puis utilise les facteurs entrés dans la règle pour chaque domaine pour calculer un score de correspondance final. Si le score de correspondance pour les deux enregistrements comparés est supérieur au score de correspondance minimal, les deux champs sont considérés comme des correspondances.  
  
 Les facteurs que vous entrez dans une règle de correspondance sont les suivants :  
  
-   Poids : pour chaque domaine de la règle, entrez un poids numérique qui détermine comment l'analyse de correspondance pour le domaine sera comparée à celle de chaque autre domaine de la règle. Le poids indique la contribution du score du champ au score de correspondance global entre deux enregistrements. Les scores calculés affectés à chaque champ source sont additionnés pour obtenir un score de correspondance composite pour les deux enregistrements. Pour chaque champ qui n'est pas un élément requis (avec Exacte ou Similaire comme valeur de similarité), définissez le poids entre 10 et 100. La somme des poids des domaines qui ne sont pas des éléments requis doit être égale à 100. Si la valeur est un élément requis, le poids a la valeur 0 et ne peut pas être modifié.  
  
-   Exacte comme valeur de similarité : sélectionnez **Exacte** si les valeurs du même domaine de deux enregistrements distincts doivent être identiques pour être considérées comme une correspondance. Si elles sont identiques, le score de correspondance pour ce domaine a la valeur « 100 », et DQS utilise ce score et les scores des autres domaines de la règle pour déterminer le score de correspondance total. Si elles ne sont pas identiques, le score de correspondance à ce domaine a la valeur « 0 », et le traitement de la règle passera à la condition suivante. Si vous configurez une règle de correspondance pour un domaine numérique et que vous sélectionnez **Similaire**, vous pouvez entrer une tolérance sous la forme d'un pourcentage ou d'un entier. Pour un domaine de type date, vous pouvez entrer une tolérance telle qu'un jour, un mois ou une année (entier) si vous sélectionnez **Similaire**; il n'y a aucune tolérance sous forme de pourcentage pour un domaine de date. Si vous sélectionnez **Exacte**, vous n'avez pas cette possibilité.  
  
-   Similaire comme valeur de similarité : sélectionnez **Similaire** si deux valeurs du même champ de deux enregistrements distincts peuvent être considérées comme une correspondance, même si elles ne sont pas identiques. Lorsque DQS exécute la règle, il calcule un score de correspondance pour ce domaine, et utilise ce score et les scores des autres domaines de la règle pour déterminer le score de correspondance total. La similarité minimale entre les valeurs d'un champ est de 60 %. Si le score de correspondance calculé pour un champ de deux enregistrements est inférieur à 60, la valeur 0 est automatiquement affectée au score de similarité. Si vous configurez une règle de correspondance pour un champ numérique et que vous sélectionnez **Similaire**, vous pouvez entrer une tolérance sous la forme d'un pourcentage ou d'un entier. Si vous configurez une règle de correspondance pour un champ de date, et que vous sélectionnez **Similaire**, vous pouvez entrer une tolérance numérique.  
  
-   Condition préalable : sélectionnez **Condition préalable** pour spécifier que les valeurs du même champ de deux enregistrements distincts doivent retourner une correspondance de 100 %, sans quoi les enregistrements ne sont pas considérés comme une correspondance et les autres clauses de la règle sont ignorées. Lorsque l'option **Condition préalable** est sélectionnée, le champ de poids du domaine est supprimé afin que vous ne puissiez pas définir un poids pour le domaine. Vous devez réinitialiser un ou plusieurs poids de domaine afin que la somme pondérée soit égale à 100. Les domaines requis ne contribuent pas au score de correspondance des enregistrements. Le score de correspondance des enregistrements est déterminé en comparant les valeurs des champs pour lesquels la similarité a la valeur Similaire ou Exact. Lorsque vous faites d'un champ une condition préalable, la valeur Exacte est automatiquement définie comme similarité pour ce domaine.  
  
 Le score de correspondance minimal est le seuil auquel ou au-delà duquel deux enregistrements sont considérés comme une correspondance (et l'état affecté aux enregistrements est « Correspondants »). Entrez une valeur entière par incréments de « 1 » ou cliquez sur la flèche haut ou bas pour augmenter ou diminuer la valeur par incréments de « 10 ». La valeur minimale est 80. Si le score de correspondance est inférieur à 80, les deux enregistrements ne sont pas considérés comme une correspondance. Vous ne pouvez pas modifier la plage du score de correspondance minimal dans cette page. Le plus faible score de correspondance minimal est 80. Vous pouvez, toutefois, modifier le plus faible score de correspondance minimal dans la page Administration (si vous êtes administrateur DQS).  
  
 La création d’une règle de correspondance est un processus itératif, car vous devrez peut-être modifier les poids relatifs des domaines dans la règle, ou bien la similarité ou la propriété requise d’un domaine, ou encore le score de correspondance minimal pour la règle, afin d’obtenir les résultats dont vous avez besoin. Vous pouvez également souhaiter créer plusieurs règles, qui seront chacune exécutées pour créer le score de correspondance. Il peut être difficile d'obtenir le résultat dont vous avez besoin avec une seule règle. Plusieurs règles fournissent des vues différentes d'une correspondance requise. Avec plusieurs règles, vous pourrez peut-être inclure moins de domaines dans chaque règle, utiliser des poids plus élevés pour chaque domaine et obtenir de meilleurs résultats. Si les données sont moins précises et moins complètes, vous pouvez avoir besoin de davantage de règles pour trouver les correspondances requises. Si les données sont plus précises et complètes, vous avez besoin de moins de règles.  
  
 Le profilage fournit des informations sur l'exhaustivité et l'unicité. Considérez l'exhaustivité et l'unicité en tandem. Utilisez les données d'exhaustivité et d'unicité pour déterminer le poids à donner à un champ dans le processus de correspondance. Si le niveau d'unicité est élevé dans un champ, l'utilisation de ce champ dans une stratégie de correspondance peut diminuer les résultats de correspondance ; de ce fait, vous pouvez affecter une valeur relativement faible pour le poids de ce champ. Si vous avez un niveau d'unicité faible pour une colonne, mais une faible exhaustivité, vous pouvez décider de ne pas inclure de domaine pour cette colonne. Avec un niveau d'unicité faible, mais un niveau d'exhaustivité élevé, vous pouvez inclure le domaine. Certaines colonnes, telles que le sexe, peuvent naturellement avoir un niveau d'unicité faible. Pour plus d’informations, consultez [Onglets Générateur de profils et Résultats](#Tabs).  
  
##  <a name="Starting"></a> Première étape : Démarrage d'une stratégie de correspondance  
 Vous effectuez l'activité de stratégie de correspondance dans la zone de gestion des bases de connaissances de l'application [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)] .  
  
1.  [!INCLUDE[ssDQSInitialStep](../includes/ssdqsinitialstep-md.md)] [Exécutez l’application Data Quality Client](../data-quality-services/run-the-data-quality-client-application.md).  
  
2.  Dans l'écran d'accueil de [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)] , cliquez sur **Nouvelle base de connaissances** pour créer une stratégie de correspondance dans une nouvelle base de connaissances. Entrez un nom pour la base de connaissances, entrez une description, puis définissez **Créer la base de connaissances à partir de** selon vos besoins. Cliquez sur **Stratégie de correspondance** pour l'activité. Cliquez sur **Suivant** pour continuer.  
  
3.  Cliquez sur **Ouvrir la base de connaissances** pour créer ou modifier la stratégie de correspondance dans une base de connaissances existante. Sélectionnez la base de connaissances, sélectionnez **Stratégie de correspondance**, puis cliquez sur **Suivant**. Vous pouvez également cliquer sur une base de connaissances sous **Base de connaissances récente**. Si vous ouvrez une base de connaissances qui a été fermée pendant qu'une stratégie de correspondance était en cours d'utilisation, vous passez alors à l'étape au cours de laquelle l'activité de stratégie de correspondance a été fermée (comme indiqué par la colonne **État** pour la base de connaissances dans la table de bases de connaissances ou dans le nom de la base de connaissances sous **Base de connaissances récente**). Si vous ouvrez une base de connaissances qui inclut une stratégie de correspondance et qui a été terminée, vous accédez à la page **Stratégie de correspondance** . Si vous ouvrez une base de connaissances qui n'inclut pas de stratégie de correspondance et qui a été terminée, vous accédez à la page **Mappage** .  
  
##  <a name="MatchingStage"></a> Étape de mappage  
 Au cours de l'étape de mappage, vous identifiez la source des données pour lesquelles vous allez créer la stratégie de correspondance, et vous mappez des colonnes sources à des domaines pour rendre les domaines disponibles pour l'activité de stratégie de correspondance.  
  
1.  Dans la page **Mapper** , pour créer une stratégie pour une base de données, laissez **SQL Server** comme valeur de **Source de données**, sélectionnez la base de données pour laquelle vous voulez créer la stratégie dans **Base de données**, puis sélectionnez la table ou la vue dans **Table/vue**. La base de données source doit être présente dans la même instance de SQL Server que [!INCLUDE[ssDQSServer](../includes/ssdqsserver-md.md)]. Sinon, elle ne s'affichera pas dans la liste déroulante.  
  
2.  Pour créer une stratégie pour les données d'une feuille de calcul Excel, sélectionnez **Fichier Excel** pour **Source de données**, cliquez sur **Parcourir** , puis sélectionnez le fichier Excel, et laissez **Utiliser la première ligne comme en-tête** sélectionné, le cas échéant. Dans **Feuille de calcul**, sélectionnez la feuille de calcul du fichier Excel qui sera la source des données. Microsoft Excel doit être installé sur l'ordinateur Data Quality Client pour sélectionner un fichier Excel. Dans le cas contraire, le bouton Parcourir n'est pas disponible, et un message sous cette zone de texte vous indique que Microsoft Excel n'est pas installé.  
  
3.  Sous **Mappages**, sélectionnez un champ pour **Colonne source**, puis cliquez sur l'icône **Créer un domaine** .  
  
4.  Sous **Mappages**, sélectionnez un champ de la source de données pour **Colonne source**, puis sélectionnez le domaine correspondant. Répétez ces étapes pour tous les domaines que vous utilisez dans le processus de correspondance. Créez des domaines, si nécessaire, en cliquant sur **Créer un domaine** ou sur **Créer un domaine composite**.  
  
    > [!NOTE]  
    >  Vous pouvez mapper vos données source à un domaine DQS lors de la création d'une stratégie de correspondance uniquement si le type de données source est pris en charge dans DQS et correspond au type de données du domaine DQS. Pour plus d'informations sur les types de données pris en charge dans DQS, consultez [Types de données SQL Server et SSIS pris en charge pour les domaines DQS](../data-quality-services/supported-sql-server-and-ssis-data-types-for-dqs-domains.md).  
  
5.  Cliquez sur le contrôle **plus (+)** pour ajouter une ligne à la table Mappages ou sur le contrôle **moins (–)** pour en supprimer une.  
  
6.  Cliquez sur **Aperçu de la source de données** pour afficher les données de la table ou de la vue SQL Server que vous avez sélectionnée, ou de la feuille de calcul Excel que vous avez sélectionnée.  
  
7.  Cliquez sur **Afficher/Sélectionner des domaines composites** pour afficher la liste des domaines composites disponibles dans la base de connaissances et pour effectuer les sélections appropriées pour le mappage.  
  
8.  Cliquez sur **Suivant** pour passer à l'étape de stratégie de correspondance.  
  
    > [!NOTE]  
    >  Cliquez sur **Fermer** pour enregistrer l'étape du projet de correspondance et revenir à la page d'accueil de DQS. La prochaine fois que vous ouvrirez ce projet, il démarrera à partir de la même étape. Cliquez sur **Annuler** pour mettre fin à l'activité de correspondance, auquel cas vous perdrez votre travail, et revenir à la page d'accueil de DQS.  
  
##  <a name="MatchingPolicyStage"></a> Étape de stratégie de correspondance  
 Vous créez des règles de correspondance et les testez individuellement dans la page Stratégie de correspondance. Lorsque vous testez une règle de correspondance dans la page **Stratégie de correspondance** , une table des résultats de correspondance indiquant les clusters identifiés par DQS pour la règle sélectionnée s'affiche. Cette table affiche chaque enregistrement du cluster avec les valeurs de domaine de mappage et le score de correspondance, ainsi que l'enregistrement pivot initial pour le cluster. Vous pouvez également afficher des données de profilage pour le processus de correspondance dans son ensemble, les conditions de chaque règle de correspondance et les statistiques sur les résultats pour chaque règle de correspondance séparément. Vous pouvez définir un filtre sur les principales données de règle de votre choix.  
  
 Pour plus d'informations sur le fonctionnement des règles de correspondance, consultez [Comment définir des paramètres de règle de correspondance](#MatchingRules).  
  
1.  Dans la page **Stratégie de correspondance** , cliquez sur l'icône **Créer une règle de correspondance** .  
  
2.  Entrez un nom et une description pour la règle.  
  
3.  Augmentez la valeur de **Score de correspondance minimal** si vous voulez rendre les exigences de correspondance plus strictes. Pour plus d'informations sur le score de correspondance minimal, consultez [Comment définir des paramètres de règle de correspondance](#MatchingRules).  
  
4.  Cliquez sur l'icône **Ajouter un nouvel élément de domaine** .  
  
5.  Sélectionnez un domaine ou un domaine composite pour lequel entrer des valeurs de règle.  
  
    > [!NOTE]  
    >  Vous pouvez sélectionner un domaine composite uniquement si chaque domaine unique du domaine composite a été mappé à une colonne source.  
  
6.  Pour **Similarité**, sélectionnez **Similaire** si deux valeurs du même champ de deux enregistrements distincts peuvent être considérées comme une correspondance, même si elles ne sont pas identiques. Sélectionnez **Exacte** si deux valeurs du même domaine de deux enregistrements distincts doivent être identiques pour être considérées comme une correspondance. (Pour plus d'informations, consultez [Comment définir des paramètres de règle de correspondance](#MatchingRules).)  
  
7.  Pour **Poids**, entrez une valeur qui détermine la contribution du score de correspondance d'un domaine au score de correspondance global pour deux enregistrements.  
  
    > [!NOTE]  
    >  Lorsque vous définissez un poids pour un domaine composite, vous pouvez entrer un poids différent pour chaque domaine unique du domaine composite, auquel cas un poids distinct n'est pas affecté au domaine composite, ou vous pouvez entrer un poids unique pour le domaine composite, auquel cas des poids distincts ne sont pas affectés aux domaines uniques du domaine composite.  
  
8.  Sélectionnez **Condition préalable** pour spécifier que les valeurs du champ de deux enregistrements doivent retourner une correspondance de 100 %, sans quoi les enregistrements ne sont pas considérés comme une correspondance et les autres clauses de la règle sont ignorées. Si la valeur de **Similarité** est **Similaire**, elle est remplacée par **Exacte**, et le poids est supprimé car la correspondance doit être de 100 %.  
  
9. Répétez les étapes 4 à 8 pour tous les autres domaines qui feront partie de la règle de correspondance. Vérifiez que la somme des poids de tous les domaines de la règle est égale à 100.  
  
10. Sélectionnez **Clusters qui se chevauchent** dans la liste déroulante pour afficher les enregistrements pivots et les enregistrements suivants pour tous les clusters lorsque la correspondance est exécutée, même si les groupes de clusters ont des enregistrements en commun. Sélectionnez **Clusters qui ne se chevauchent pas** pour afficher les clusters qui ont des enregistrements en commun comme un seul cluster lorsque la correspondance est exécutée.  
  
11. Cliquez sur **Recharger les données à partir de la source** pour copier les données de la source de données vers la table de mise en lots et les réindexer lorsque vous exécutez la stratégie de correspondance. Cliquez sur **Exécuter sur les données précédentes** pour exécuter une stratégie de correspondance sans copier les données vers la table de mise en lot et les réindexer. L'option**Exécuter sur les données précédentes** est désactivée pour la première exécution de la stratégie de correspondance, ou si vous modifiez le mappage dans la page **Mapper** et que vous appuyez sur **Oui** dans la fenêtre suivante. Dans ces deux cas, vous devez effectuer une réindexation. Il n'est pas nécessaire d'effectuer une réindexation si la stratégie de correspondance n'a pas changé. L'exécution sur les données précédentes peut améliorer les performances.  
  
12. Cliquez sur **Démarrer** pour exécuter le processus de correspondance pour la règle sélectionnée. Lorsque le processus est terminé, la table affiche l'ID d'enregistrement, le numéro de cluster et les colonnes de données (y compris celles ne figurant pas dans la règle de correspondance) pour chaque enregistrement d'un cluster. La ligne pivot du cluster est considérée comme le candidat principal pour la survivance du processus de déduplication. Chaque ligne supplémentaire d'un cluster est considérée comme un doublon ; son score de correspondance (comparé à l'enregistrement pivot) est fourni dans la table des résultats. Le numéro de cluster est identique à l'ID d'enregistrement pour l'enregistrement principal du cluster.  
  
13. Vous pouvez utiliser les données de la table **Résultats de correspondance** comme suit :  
  
    -   Dans **Filtre**, sélectionnez **Correspondants** pour afficher toutes les lignes correspondantes et leur score. Les lignes qui ne sont pas considérées comme des correspondances (c'est-à-dire qui ont un score de correspondance inférieur au score de correspondance minimal) ne sont pas affichées dans la table des résultats de correspondance. Sélectionnez **Sans correspondance** pour afficher toutes les lignes sans correspondance.  
  
    -   Dans la zone de liste déroulante **Pourcentage**, sélectionnez un pourcentage, par incréments de « 5 ». Toutes les lignes ayant un score de correspondance supérieur ou égal à ce pourcentage seront affichées dans la table des résultats de correspondance.  
  
    -   Si vous double-cliquez sur un enregistrement dans la table des résultats de correspondance, DQS affiche une fenêtre **Détails du score de correspondance** qui présente l'enregistrement pivot et l'enregistrement source (ainsi que les valeurs de tous leurs champs), le score entre elles et une exploration de correspondance d'enregistrement. L'exploration affiche les valeurs de chaque champ de l'enregistrement pivot et de l'enregistrement source pour vous permettre de les comparer, et indique le score de correspondance auquel chaque champ contribue dans le score de correspondance global pour les deux enregistrements.  
  
14. Affichez les statistiques sous les onglets **Générateur de profils** et **Résultats de correspondance** pour vérifier que vous obtenez les résultats dont vous avez besoin. Pour plus d’informations, consultez [Profiler and Results Tabs](#Tabs).  
  
15. Si la règle doit être modifiée, effectuez cette opération dans l'éditeur de règles, puis cliquez sur **Redémarrer**.  
  
    > [!NOTE]  
    >  Une fois la première analyse terminée, le bouton **Démarrer** devient le bouton **Redémarrer** . Si les résultats de l'analyse précédente n'ont pas encore été enregistrés, le fait de cliquer sur **Redémarrer** entraîne la perte de ces données précédentes. Pendant l'exécution de l'analyse, ne quittez pas la page, car le processus d'analyse s'arrête alors.  
  
16. L'onglet **Résultats de correspondance** affiche des statistiques pour les deux dernières exécutions de la règle. Si vous avez exécuté la règle de correspondance plusieurs fois avec des paramètres différents, comparez les statistiques pour la règle actuelle et la règle précédente. Si vous trouvez que les résultats de la règle précédente étaient meilleurs, cliquez sur **Restaurer la règle antérieure** pour restaurer les conditions de la règle précédente, ce qui remet la règle dans son état précédent avant la modification. Les conditions de règle actuelles seront perdues. Cela vous permet de paramétrer la stratégie en fonction des deux dernières exécutions de correspondance, ce qui diminue le temps nécessaire pour paramétrer la stratégie de correspondance.  
  
17. Si vous voulez qu'une autre règle soit ajoutée à la stratégie de correspondance, répétez l'opération à partir de l'étape 1.  
  
18. Cliquez sur **Suivant** pour passer à l'étape des résultats de correspondance.  
  
##  <a name="MatchingResultsStage"></a> Étape des résultats de correspondance  
 Vous testez toutes vos règles de correspondance simultanément dans la page **Résultats de correspondance** . Avant cela, vous pouvez spécifier que l'exécution des tests de règle identifie les clusters qui se chevauchement ou qui ne se chevauchent pas. Si vous exécutez les règles plusieurs fois, vous pouvez exécuter la règle sur des données rechargées à partir de la source ou sur les données précédentes.  
  
 Lorsque vous testez les règles de correspondance dans la page **Résultats de correspondance** , une table des résultats de correspondance indiquant les clusters identifiés par DQS pour toutes les règles s'affiche. Cette table affiche chaque enregistrement du cluster avec les valeurs de domaine de mappage et le score de correspondance, ainsi que l'enregistrement pivot initial pour le cluster. Vous pouvez également afficher des données de profilage pour les règles de correspondance dans leur ensemble, les conditions de chaque règle de correspondance et les statistiques sur les résultats pour toutes les règles de correspondance.  
  
1.  Sur la page **Résultats de correspondance** , sélectionnez **Clusters qui se chevauchent** dans la liste déroulante pour afficher les enregistrements pivots et les enregistrements suivants pour tous les clusters lorsque la correspondance est exécutée, même si les groupes de clusters ont des enregistrements en commun. Sélectionnez **Clusters qui ne se chevauchent pas** pour afficher les clusters qui ont des enregistrements en commun comme un seul cluster lorsque la correspondance est exécutée.  
  
2.  Cliquez sur **Recharger les données à partir de la source** pour copier les données de la source de données vers la table de mise en lots et les réindexer lorsque vous exécutez la stratégie de correspondance. Cliquez sur **Exécuter sur les données précédentes** pour exécuter une stratégie de correspondance sans copier les données vers la table de mise en lot et les réindexer. L'option**Exécuter sur les données précédentes** est désactivée pour la première exécution de la stratégie de correspondance, ou si vous modifiez le mappage dans la page **Mapper** et que vous appuyez sur **Oui** dans la fenêtre suivante. Dans ces deux cas, vous devez effectuer une réindexation. Il n'est pas nécessaire d'effectuer une réindexation si la stratégie de correspondance n'a pas changé. L'exécution sur les données précédentes peut améliorer les performances.  
  
3.  Cliquez sur **Démarrer** pour exécuter le processus de correspondance pour toutes les règles que vous avez définies. La table **Résultats de correspondance** affiche l'ID d'enregistrement, le numéro de cluster et les colonnes de données (y compris celles ne figurant pas dans la règle de correspondance) pour chaque enregistrement d'un cluster. L'enregistrement de début du cluster est sélectionné de manière aléatoire. (Vous déterminez l’enregistrement survivant en sélectionnant la règle de survivance dans la page **Exporter** quand vous exécutez le projet de correspondance.) Chaque ligne supplémentaire d'un cluster est considérée comme un doublon ; son score de correspondance (comparé à l'enregistrement pivot) est fourni dans la table des résultats.  
  
4.  Vous pouvez utiliser les données de la table **Résultats de correspondance** comme suit :  
  
    -   Dans **Filtre**, sélectionnez **Correspondants** pour afficher toutes les lignes correspondantes et leur score. Les lignes qui ne sont pas considérées comme des correspondances (c'est-à-dire qui ont un score de correspondance inférieur au score de correspondance minimal) ne sont pas affichées dans la table des résultats de correspondance. Sélectionnez **Sans correspondance** pour afficher toutes les lignes sans correspondance.  
  
    -   Dans la zone de liste déroulante **Pourcentage**, sélectionnez un pourcentage, par incréments de « 5 ». Toutes les lignes ayant un score de correspondance supérieur ou égal à ce pourcentage seront affichées dans la table des résultats de correspondance.  
  
    -   Si vous double-cliquez sur un enregistrement dans la table des résultats de correspondance, DQS affiche une fenêtre **Détails du score de correspondance** qui présente l'enregistrement pivot et l'enregistrement source (ainsi que les valeurs de tous leurs champs), le score entre elles et une exploration de correspondance d'enregistrement. L'exploration affiche les valeurs de chaque champ de l'enregistrement pivot et de l'enregistrement source pour vous permettre de les comparer, et indique le score de correspondance auquel chaque champ contribue dans le score de correspondance global pour les deux enregistrements.  
  
5.  Affichez les statistiques sous les onglets **Générateur de profils** et **Résultats de correspondance** pour vérifier que vous obtenez les résultats dont vous avez besoin. Cliquez sur l'onglet **Règles de correspondance** pour voir quels sont les paramètres de domaine pour chaque règle. Pour plus d’informations, consultez [Onglets Générateur de profils et Résultats](#Tabs).  
  
6.  Si vous n'êtes pas satisfait des résultats obtenus avec toutes les règles, cliquez sur **Précédent** pour revenir à la page **Stratégie de correspondance** , modifiez une ou plusieurs règles en fonction des besoins, retournez à la page **Résultats de correspondance** , puis cliquez sur **Redémarrer**.  
  
    > [!NOTE]  
    >  Une fois l'analyse terminée, le bouton **Démarrer** se transforme en bouton **Redémarrer** . Si les résultats de l'analyse précédente n'ont pas encore été enregistrés, le fait de cliquer sur **Redémarrer** entraîne la perte de ces données précédentes.  
  
7.  Si vous êtes satisfait des résultats de toutes les règles, cliquez sur **Terminer** pour terminer le processus de stratégie de correspondance, puis cliquez sur l'une des options suivantes :  
  
    -   **Oui. Publier la base de connaissances et quitter**: la base de connaissances sera publiée en vue de son utilisation par l'utilisateur actuel ou d'autres. La base de connaissances ne sera pas verrouillée, l'état de la base de connaissances (dans la table de bases de connaissances) sera défini sur Vide, et les activités de gestion de l'arborescence du domaine et de découverte des connaissances seront disponibles. L'écran Ouvrir la base de connaissances s'affichera à nouveau.  
  
    -   **Non. Enregistrer le travail dans la base de connaissances et quitter**: votre travail sera enregistré, la base de connaissances restera verrouillée et l'état de la base de connaissances sera défini sur **En cours**. Les activités de gestion de l'arborescence du domaine et de découverte des connaissances seront disponibles. La page d'accueil s'affichera à nouveau.  
  
    -   **Annuler. Rester sur l'écran actif**: la fenêtre sera fermée et l'écran de gestion de l'arborescence du domaine s'affichera à nouveau.  
  
8.  Cliquez sur **Fermer** pour enregistrer votre travail et revenir à la page d'accueil de DQS. L'état de la base de connaissances affiche la chaîne « Stratégie de correspondance – », ainsi que l'état actuel. Si vous cliquez sur **Fermer** lorsque vous êtes dans l'écran **Résultats de correspondance** , l'état affiche : « Stratégie de correspondance - Résultats ». Si vous cliquez sur Fermer lorsque vous êtes dans l'écran **Stratégie de correspondance** , l'état affiche : « Stratégie de correspondance - Stratégie de correspondance ». Après avoir cliqué sur **Fermer**, pour effectuer l'activité **Découverte des connaissances** , revenez à l'activité **Stratégie de correspondance** , cliquez sur **Terminer**, puis sur **Oui** pour publier la base de connaissances ou sur **Non** pour enregistrer le travail dans la base de connaissances et quitter.  
  
    > [!NOTE]  
    >  Si vous cliquez sur **Fermer** alors qu'un processus de correspondance est en cours d'exécution, ce dernier ne se termine pas lorsque vous cliquez sur **Fermer**. Vous pouvez rouvrir la base de connaissances et voir soit que le processus est toujours en cours d'exécution, soit, s'il est terminé, que les résultats sont affichés. Si le processus n'est pas terminé, l'écran affiche la progression.  
  
9. Cliquez sur **Annuler** pour mettre fin à l'activité Stratégie de correspondance, auquel cas vous perdrez votre travail, et revenir à la page d'accueil de DQS.  
  
##  <a name="FollowUp"></a> Suivi : Après avoir créé une stratégie de correspondance  
 Après avoir créé une stratégie de correspondance, vous pouvez exécuter un projet de correspondance basé sur la base de connaissances qui contient la stratégie de correspondance. Pour plus d’informations, consultez [Exécuter un projet de correspondance](../data-quality-services/run-a-matching-project.md).  
  
##  <a name="Tabs"></a> Profiler and Results Tabs  
 Les onglets Générateur de profils et Résultats contiennent des statistiques pour les pages Stratégie de correspondance et Résultats de correspondance.  
  
###  <a name="Profiler"></a> Onglet Générateur de profils  
 Cliquez sur l'onglet **Générateur de profils** pour afficher des statistiques pour la base de données source et pour chaque champ inclus dans la règle de stratégie. Les statistiques sont mises à jour lorsque la règle de stratégie est exécutée.  
  
 Pour plus d'informations sur l'interprétation des statistiques suivantes, consultez [Comment définir des paramètres de règle de correspondance](#MatchingRules).  
  
 Les statistiques de la base de données source sont les suivantes :  
  
-   **Enregistrements**: nombre total d'enregistrements dans la base de données source  
  
-   **Valeurs totales**: nombre total de valeurs dans les champs de la source de données  
  
-   **Nouvelles valeurs**: nombre total de valeurs qui sont nouvelles depuis l'exécution précédente, et leur pourcentage par rapport à la totalité  
  
-   **Valeurs uniques**: nombre total de valeurs uniques dans les champs, et leur pourcentage par rapport à la totalité  
  
-   **Nouvelles valeurs uniques**: nombre total de valeurs uniques qui sont nouvelles dans les champs, et leur pourcentage par rapport à la totalité  
  
 Les statistiques de champ sont les suivantes :  
  
-   **Nom du champ**  
  
-   **Nom de domaine**  
  
-   **Nouveau**: nombre de nouvelles valeurs et pourcentage de valeurs nouvelles comparées aux valeurs existantes dans le domaine  
  
-   **Unique**: nombre d'enregistrements uniques dans le champ et leur pourcentage par rapport au total  
  
-   **Exhaustivité**: exhaustivité de chaque champ source qui est mappé pour l'exercice correspondant  
  
###  <a name="Notifications"></a> Notifications de stratégie de correspondance  
 Pour l'activité de stratégie de correspondance, les conditions suivantes génèrent des notifications :  
  
-   Le champ est vide dans tous les enregistrements ; il est recommandé de l'éliminer du mappage.  
  
-   Le score d'achèvement du champ est très faible ; vous pouvez l'éliminer du mappage.  
  
-   Toutes les valeurs d'un champ sont non valides ; vous devez vérifier le mappage et la pertinence des règles de domaine par rapport au contenu du champ.  
  
-   Il existe un faible niveau de valeurs valides dans le champ ; vous devez vérifier le mappage et la pertinence des règles de domaine par rapport au contenu du champ.  
  
-   Il existe un niveau élevé d'unicité dans ce champ. L'utilisation de ce champ dans la stratégie de correspondance peut diminuer les résultats de correspondance.  
  
###  <a name="ResultsTab"></a> Onglet Résultats de correspondance  
 Cliquez sur l'onglet **Résultats de correspondance** pour afficher des statistiques pour l'exécution de règles de stratégie de correspondance, et l'exécution de règles précédente. Si vous avez exécuté la même règle plusieurs fois avec des valeurs différentes, la table des résultats de correspondance affiche des statistiques pour les deux exécutions, ce qui vous permet de les comparer. Vous pouvez également restaurer la règle précédente si vous souhaitez.  
  
 Les statistiques sont les suivantes :  
  
-   Nombre total d'enregistrements dans la base de données  
  
-   Nombre total d'enregistrements correspondants dans la base de données  
  
-   Nombre d'enregistrements dans la base de données qui ne sont pas considérés comme des doublons  
  
-   Nombre de clusters découverts  
  
-   Taille de cluster moyenne (nombre d'enregistrements en double divisé par le nombre de clusters)  
  
-   Nombre le moins élevé de doublons dans un cluster  
  
-   Nombre le plus élevé de doublons dans un cluster  
  
  
