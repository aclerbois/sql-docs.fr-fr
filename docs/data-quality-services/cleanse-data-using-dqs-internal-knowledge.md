---
title: Nettoyer des données à l’aide de la base de connaissances DQS (interne) | Microsoft Docs
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
- sql13.dqs.dqproject.interactivecleansing.f1
- sql13.dqs.dqproject.map.f1
- sql13.dqs.dqproject.correction.f1
- sql13.dqs.dqproject.export.f1
ms.assetid: c96b13ad-02a6-4646-bcc7-b4a8d490f5cc
caps.latest.revision: 26
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 70f7975e2bf6408239bd4d4fe7c07af169e0bf29
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="cleanse-data-using-dqs-internal-knowledge"></a>Nettoyer des données à l'aide de la base de connaissances DQS (interne)

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  Cette rubrique explique comment nettoyer vos données en utilisant un projet de qualité des données dans [!INCLUDE[ssDQSnoversion](../includes/ssdqsnoversion-md.md)] (DQS). Le nettoyage des données est effectué sur vos données sources à l'aide d'une base de connaissances générée dans DQS sur un ensemble de données de haute qualité. Pour plus d’informations, consultez [Construction d’une base de connaissances](../data-quality-services/building-a-knowledge-base.md).  
  
 Le nettoyage des données s'effectue en quatre étapes : une étape de *mappage* au cours de laquelle vous identifiez la source de données à nettoyer et la mappez aux domaines requis dans une base de connaissances, une étape de *nettoyage assisté par ordinateur* au cours de laquelle DQS applique la base de connaissances aux données à nettoyer et propose/apporte des modifications pour les données sources, une étape de *nettoyage interactif* au cours de laquelle des gestionnaires de données peuvent analyser les modifications de données, et les accepter ou les refuser, et enfin l'étape d' *exportation* qui vous permet d'exporter les données nettoyées. Chacun de ces processus est effectué sur une page distincte de l'Assistant de l'activité de nettoyage, ce qui vous permet de naviguer entre les différentes pages, de réexécuter le processus et de fermer un processus de nettoyage spécifique, puis de retourner à la même étape du processus. DQS vous fournit des statistiques sur les données sources et les résultats du nettoyage, lesquelles vous permettent de prendre des décisions avisées sur le nettoyage des données.  
  
## <a name="before-you-begin"></a>Avant de commencer  
  
###  <a name="Prerequisites"></a> Conditions préalables  
  
-   Vous devez avoir spécifié les valeurs de seuil appropriées pour l'activité de nettoyage. Pour plus d'informations sur cette opération, consultez [Configurer les valeurs de seuil pour le nettoyage et la correspondance](../data-quality-services/configure-threshold-values-for-cleansing-and-matching.md).  
  
-   Une base de connaissances DQS à utiliser pour comparer et nettoyer vos données sources doit être disponible sur [!INCLUDE[ssDQSServer](../includes/ssdqsserver-md.md)] . En outre, la base de connaissances doit contenir la connaissance sur le type de données que vous souhaitez nettoyer. Par exemple, si vous voulez nettoyer vos données sources qui contiennent des adresses des États-Unis, vous devez disposer d'une base de connaissances créée avec un échantillon de données de « haute qualité » pour les adresses des États-Unis.  
  
-   Microsoft Excel doit être installé sur l'ordinateur [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)] si les données sources à nettoyer se trouvent dans un fichier Excel. Sinon, vous ne pourrez pas sélectionner le fichier Excel à l'étape de mappage. Les fichiers créés par Microsoft Excel peuvent avoir une extension .xlsx, .xls ou .csv. Si la version 64 bits d'Excel est utilisée, seuls les fichiers Excel 2003 (.xls) sont pris en charge ; les fichiers Excel 2007 ou 2010 (.xlsx) ne sont pas pris en charge. Si vous utilisez la version 64 bits d'Excel 2007 ou 2010, enregistrez le fichier comme fichier .xls ou fichier .csv, ou installez une version 32 bits d'Excel à la place.  
  
###  <a name="Security"></a> Sécurité  
  
####  <a name="Permissions"></a> Permissions  
 Vous devez disposer du rôle dqs_kb_editor ou dqs_kb_operator sur la base de données DQS_MAIN pour effectuer le nettoyage des données.  
  
##  <a name="Create"></a> Créer un projet de qualité des données pour le nettoyage  
 Vous devez utiliser un projet de qualité des données pour effectuer une opération de nettoyage des données. Pour créer un projet de qualité des données pour le nettoyage :  
  
1.  Suivez les étapes 1 à 3 dans la rubrique [Créer un projet de qualité des données](../data-quality-services/create-a-data-quality-project.md).  
  
2.  À l'étape 3.d, sélectionnez l'activité **Nettoyage** .  
  
3.  Cliquez sur **Créer** pour créer un projet de qualité des données pour le nettoyage.  
  
 Cela entraîne la création d'un projet de qualité des données pour le nettoyage, et ouvrez la page **Mapper** de l'Assistant de qualité des données pour le nettoyage.  
  
##  <a name="Mapping"></a> Étape de mappage  
 Au cours de l'étape de mappage, vous spécifiez la connexion aux données sources à nettoyer, et mappez les colonnes des données sources avec les domaines appropriés dans la base de connaissances sélectionnée.  
  
1.  Dans la page **Mapper** de l'Assistant de qualité des données pour le nettoyage, sélectionnez vos données sources à nettoyer : **SQL Server** ou **Fichier Excel**:  
  
    1.  **SQL Server**: sélectionnez **DQS_STAGING_DATA** comme base de données source si vous avez copié vos données sources dans cette base de données, puis sélectionnez la table/vue appropriée qui contient vos données sources. Sinon, sélectionnez votre base de données source et la table/vue appropriée. Pour être disponible dans la liste déroulante [!INCLUDE[ssDQSServer](../includes/ssdqsserver-md.md)] Base de données **, votre base de données source doit être présente dans la même instance de SQL Server que** .  
  
    2.  **Fichier Excel**: cliquez sur **Parcourir**, puis sélectionnez le fichier Excel qui contient les données à nettoyer. Pour pouvoir sélectionner un fichier Excel, Microsoft Excel doit être installé sur l'ordinateur [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)] . Dans le cas contraire, le bouton **Parcourir** n'est pas disponible, et un message sous cette zone de texte vous indique que Microsoft Excel n'est pas installé. De plus, laissez la case à cocher **Utiliser la première ligne comme en-tête** activée si la première ligne du fichier Excel contient des données d'en-tête.  
  
2.  Sous **Mappages**, mappez les colonnes de données de vos données sources avec les domaines appropriés dans la base de connaissances en sélectionnant une colonne source dans la liste déroulante de la colonne **Colonne source** , puis en sélectionnant un domaine dans la liste déroulante de la colonne **Domaine** de la même ligne. Répétez cette étape pour mapper toutes les colonnes de vos données sources avec les domaines appropriés dans la base de connaissances. Si nécessaire, vous pouvez cliquer sur l'icône **Ajouter un mappage de colonnes** pour ajouter des lignes à la table de mappage.  
  
    > [!NOTE]  
    >  Vous pouvez mapper vos données source à un domaine DQS pour effectuer un nettoyage des données uniquement si le type de données source est pris en charge dans DQS et correspond au type de données du domaine DQS. Pour plus d'informations sur les types de données source pris en charge, consultez [Types de données SQL Server et SSIS pris en charge pour les domaines DQS](../data-quality-services/supported-sql-server-and-ssis-data-types-for-dqs-domains.md).  
  
3.  Cliquez sur l'icône **Aperçu de la source de données** pour afficher les données de la table ou vue SQL Server que vous avez sélectionnée, ou la feuille de calcul Excel que vous avez sélectionnée.  
  
4.  Cliquez sur **Afficher/Sélectionner des domaines composites** pour afficher la liste des domaines composites mappés à une colonne source. Ce bouton est disponible uniquement si au moins un domaine composite est mappé à une colonne source.  
  
5.  Cliquez sur **Suivant** pour passer à l'étape de nettoyage assisté par ordinateur (page**Nettoyer** ).  
  
##  <a name="ComputerAssisted"></a> Étape de nettoyage assisté par ordinateur  
 Au cours de l'étape de nettoyage assisté par ordinateur, vous exécutez un processus de nettoyage des données automatisé qui analyse les données sources par rapport aux domaines mappés de la base de connaissances, puis apporte/propose des modifications des données.  
  
1.  Dans la page **Nettoyer** de l'Assistant de qualité des données, cliquez sur **Démarrer** pour exécuter le processus de nettoyage assisté par ordinateur. DQS utilise des algorithmes avancés et des niveaux de confiance basés sur les niveaux de seuil spécifiés pour analyser vos données par rapport à la base de connaissances sélectionnée, puis les nettoyer. Pour plus d’informations sur le processus de nettoyage assisté par ordinateur dans DQS, consultez [Nettoyage assisté par ordinateur](../data-quality-services/data-cleansing.md#ComputerAssisted) dans [Nettoyage des données](../data-quality-services/data-cleansing.md).  
  
    > [!IMPORTANT]  
    >  -   Une fois l'analyse des données terminée, le bouton **Démarrer** devient le bouton **Redémarrer** . Si les résultats de l'analyse précédente n'ont pas encore été enregistrés, le fait de cliquer sur **Redémarrer** entraîne la perte de ces données précédentes. Pendant l'exécution de l'analyse, ne quittez pas la page, car le processus d'analyse s'arrête alors.  
    > -   Si la base de connaissances utilisée pour le projet de nettoyage a été mise à jour et publiée après le moment de création du projet de nettoyage, le fait de cliquer sur **Démarrer** entraîne l'affichage d'un message vous demandant si la base de connaissances la plus récente doit être utilisée pour le nettoyage. Cela peut généralement se produire si vous avez créé un projet de qualité des données à l'aide d'une base de connaissances, fermé le projet de nettoyage à mi-chemin en cliquant sur **Fermer**, puis rouvert le projet de qualité des données ultérieurement pour effectuer le nettoyage. Entretemps, la base de connaissances utilisée dans le projet de nettoyage a été mise à jour et publiée.  
    >   
    >      De même, si la base de connaissances utilisée pour le projet de nettoyage a été mise à jour et publiée après la dernière exécution du nettoyage assisté par ordinateur, le fait de cliquer sur **Redémarrer** entraîne l'affichage d'un message vous demandant si la base de connaissances la plus récente doit être utilisée pour le nettoyage.  
    >   
    >      Dans les deux cas, cliquez sur **Oui** pour utiliser la base de connaissances mise à jour pour le nettoyage assisté par ordinateur. En outre, s'il y a des conflits entre les mappages actuels et la base de connaissances mise à jour (tels que des domaines supprimés ou un type de données de domaine modifié), le message vous demande également de corriger les mappages actuels pour utiliser la base de connaissances mise à jour. Le fait de cliquer sur **Oui** permet d'accéder à la page **Mapper** où vous pouvez corriger les mappages avant de poursuivre le nettoyage assisté par ordinateur.  
  
2.  Au cours de l'étape de nettoyage assisté par ordinateur, vous pouvez basculer vers le Générateur de profils en cliquant sur l'onglet **Générateur de profils** pour afficher les notifications et le profilage des données en temps réel. Pour plus d’informations, consultez [Statistiques du Générateur de profils](#Profiler).  
  
3.  Si vous n'êtes pas satisfait des résultats obtenus, cliquez sur **Précédent** pour revenir à la page **Mapper** , modifiez un ou plusieurs mappages en fonction des besoins, retournez à la page **Nettoyer** , puis cliquez sur **Redémarrer**.  
  
4.  Une fois le processus de nettoyage assisté par ordinateur terminé, cliquez sur **Suivant** pour passer à l'étape de nettoyage interactif (page**Gérer et afficher les résultats** ).  
  
##  <a name="Interactive"></a> Étape de nettoyage interactif  
 Au cours de l'étape de nettoyage interactif, vous pouvez voir les modifications que DQS a proposées, et décider de les implémenter, ou non, en les approuvant ou en les refusant. Dans le volet gauche de la page **Gérer et afficher les résultats** , DQS affiche la liste de tous les domaines que vous avez mappés précédemment au cours de l'étape de mappage, avec le nombre de valeurs des données sources analysées dans chaque domaine pendant l'étape de nettoyage assistée par ordinateur. Dans le volet droit de la page **Gérer et afficher les résultats** , en fonction du respect des règles de domaine, les règles de syntaxe et les algorithmes avancés, DQS classe les données sous cinq onglets avec un *niveau de confiance*. Le niveau de confiance indique le degré de certitude de DQS quant à la correction ou à la suggestion, et est basé sur les valeurs de seuil suivantes :  
  
-   **Seuil de correction automatique**: toute valeur qui a un niveau de confiance au-dessus de ce seuil est automatiquement corrigée par DQS. Toutefois, le gestionnaire de données peut remplacer la modification au cours du nettoyage interactif. Vous pouvez spécifier la valeur de seuil de correction automatique sous l'onglet **Paramètres généraux** de l'écran **Configuration** . Pour plus d’informations, consultez [Configurer les valeurs de seuil pour le nettoyage et la correspondance](../data-quality-services/configure-threshold-values-for-cleansing-and-matching.md).  
  
-   **Seuil de suggestion automatique**: toute valeur qui a un niveau de confiance au-dessus de ce seuil, mais en dessous du seuil de correction automatique, est suggérée en tant que valeur de remplacement. DQS apportera la modification uniquement si le gestionnaire de données l'approuve. Vous pouvez spécifier la valeur de seuil de suggestion automatique sous l'onglet **Paramètres généraux** de l'écran **Configuration** . Pour plus d’informations, consultez [Configurer les valeurs de seuil pour le nettoyage et la correspondance](../data-quality-services/configure-threshold-values-for-cleansing-and-matching.md).  
  
-   **Autre**: toute valeur en dessous de la valeur de seuil de suggestion automatique n'est pas modifiée par DQS.  
  
 En fonction du niveau de confiance, les valeurs sont affichée sous les cinq onglets suivants :  
  
|Onglet|Description|  
|---------|-----------------|  
|**Suggérés**|Affiche les valeurs de domaine pour lesquelles DQS a identifié les valeurs suggérées dont le niveau de confiance est supérieur à la valeur du *seuil de suggestion automatique* mais inférieur à la valeur du *seuil de correction automatique* .<br /><br /> Les valeurs suggérées sont affichées dans la colonne **Corriger vers** de la valeur d'origine. Vous pouvez cliquer sur la case d'option dans la colonne **Approuver** ou **Refuser** d'une valeur de la grille supérieure pour accepter ou refuser la suggestion pour toutes les instances de la valeur. Dans ce cas, la valeur acceptée se déplace vers l'onglet **Corrigés** et la valeur refusée se déplace vers l'onglet **Non valide** .|  
|**Nouveau**|Affiche les valeurs de domaine valides pour lesquelles DQS ne dispose pas de suffisamment d'informations et qui ne peuvent donc pas être mappées à aucun autre onglet. En outre, cet onglet contient également les valeurs dont le niveau de confiance est inférieur à la valeur du *seuil de suggestion automatique* , mais suffisamment élevé pour être considéré comme valide.<br /><br /> Si vous pensez que la valeur est correcte, cliquez sur la case d'option située dans la colonne **Approuver** . Sinon, cliquez sur la case d'option située dans la colonne **Refuser** . La valeur acceptée se déplace vers l'onglet **Correct** et la valeur refusée se déplace vers l'onglet **Non valide** . Vous pouvez également taper la valeur correcte à la place de la valeur d’origine dans la colonne **Corriger vers** de la valeur, puis cliquer sur la case d’option située dans la colonne **Approuver** pour accepter la modification. Dans ce cas, la valeur se déplace vers l'onglet **Corrigés** .|  
|**Non valide**|Affiche les valeurs de domaine qui ont été marquées comme non valides dans le domaine de la base de connaissances ou les valeurs qui n'ont pas respecté une règle de domaine. Cet onglet contient également les valeurs qui ont été refusées par l'utilisateur sous l'un des quatre autres onglets.<br /><br /> Toutefois, si vous pensez que la valeur est correcte, cliquez sur la case d'option située dans la colonne **Approuver** . La valeur acceptée se déplace vers l'onglet **Correct** . Vous pouvez également taper la valeur correcte à la place de la valeur d’origine dans la colonne **Corriger vers** de la valeur, puis cliquer sur la case d’option située dans la colonne **Approuver** pour accepter la modification. Dans ce cas, la valeur se déplace vers l'onglet **Corrigés** .|  
|**Corrigés**|Affiche les valeurs de domaine corrigées par DQS pendant le processus de nettoyage automatisé lorsque DQS a trouvé une correction pour la valeur dont le niveau de confiance est supérieur à la valeur du seuil de correction automatique.<br /><br /> Les valeurs corrigées sont affichées dans la colonne **Corriger vers** de la valeur d'origine. Par défaut, la case d'option située dans la colonne **Approuver** de la valeur est sélectionnée. Si nécessaire, vous pouvez refuser la correction proposée en cliquant sur la case d'option située dans la colonne **Refuser** pour la déplacer vers l'onglet **Non valide** , ou tapez manuellement la valeur correcte dans la colonne **Corriger vers** puis cliquez sur la case d'option de la colonne **Approuver** pour accepter la modification et la déplacer vers l'onglet **Corrigés** .|  
|**Correct**|Affiche les valeurs de domaine qui ont été signalés comme étant correctes. Par exemple, la valeur correspond à une valeur de domaine. Cet onglet contient également des valeurs qui ont été approuvées par l'utilisateur en cliquant sur la case d'option de la colonne **Approuver** sous les onglets **Nouveau** et **Non valide** .<br /><br /> Par défaut, la case d'option située dans la colonne **Approuver** est sélectionnée pour chaque valeur. Toutefois, si vous pensez qu'une valeur de cet onglet est incorrecte, vous pouvez soit cliquer sur la case d'option située dans la colonne **Refuser** de la valeur pour la déplacer vers l'onglet **Non valide** , soit taper manuellement la valeur correcte en remplacement de la valeur affichée dans la colonne **Corriger vers** de la valeur, puis cliquer sur la case d'option de la colonne **Approuver** pour accepter la modification et la déplacer vers l'onglet **Corrigés** .|  
  
 Pour nettoyer les données de manière interactive :  
  
1.  Dans la page **Gérer et afficher les résultats** de l'Assistant de qualité des données pour le nettoyage, cliquez sur un nom de domaine dans le volet gauche.  
  
2.  Examinez les valeurs de domaine affichées sous les cinq onglets, puis prenez la mesure appropriée, comme expliqué précédemment.  
  
    -   Le volet supérieur droit affiche les informations suivantes pour chaque valeur du domaine sélectionné : la valeur d'origine, le nombre d'instances (enregistrements), une zone pour spécifier une autre valeur (correcte), le niveau de confiance (non disponible pour les valeurs affichées sous l'onglet **Correct** ), la raison de l'action de DQS sur la valeur, ainsi que l'option permettant d'approuver et de refuser les corrections et les suggestions pour la valeur.  
  
        > [!TIP]  
        >  Vous pouvez approuver ou refuser toutes les valeurs du domaine sélectionné dans le volet supérieur droit en cliquant sur l'icône **Approuver tous les termes** ou **Refuser tous les termes** , respectivement. Vous pouvez également cliquer avec le bouton droit sur une valeur dans le domaine sélectionné, puis cliquer sur **Accepter tout** ou **Refuser tout** dans le menu contextuel.  
  
    -   Le volet inférieur affiche des occurrences individuelles de la valeur de domaine sélectionnée dans le volet supérieur droit. Les informations suivantes sont affichées : une zone pour spécifier une autre valeur (correcte), le niveau de confiance (non disponible pour les valeurs affichées sous l'onglet **Correct** ), la raison de l'action de DQS sur la valeur, l'option permettant d'approuver et de refuser les corrections et les suggestions pour la valeur, ainsi que la valeur d'origine.  
  
3.  Si vous avez activé la fonctionnalité **Vérificateur d'orthographe** pour un domaine lors de sa création, des traits de soulignement ondulés rouges s'affichent pour de telles valeurs de domaine qui sont identifiées comme une erreur potentielle. Le trait de soulignement est affiché pour la totalité de la valeur. Par exemple, si « New York » est mal orthographié en tant que « Neu York », le vérificateur d'orthographe affiche un trait de soulignement rouge sous « Neu York », et pas seulement sous « Neu ». Si vous cliquez avec le bouton droit sur la valeur, des suggestions de corrections s'affichent. S'il existe plus de 5 suggestions, vous pouvez cliquer sur **Plus de suggestions** dans le menu contextuel pour afficher celles restantes. Comme pour l'affichage des erreurs, les suggestions sont des remplacements de la totalité de la valeur. Par exemple, dans l'exemple précédent, « New York » sera affiché comme suggestion, et pas simplement « New ». Vous pouvez choisir l'une des suggestions ou ajouter au dictionnaire une valeur à afficher pour cette valeur. Les valeurs sont stockées en le dictionnaire à un niveau de compte d'utilisateur. Lorsque vous sélectionnez une suggestion dans le menu contextuel du vérificateur d'orthographe, la suggestion sélectionnée est ajoutée à la colonne **Corriger vers** . Toutefois, si vous sélectionnez une suggestion dans la colonne **Corriger vers** , la valeur de la colonne est remplacée par la suggestion sélectionnée.  
  
     La fonctionnalité de vérificateur d'orthographe est activée par défaut à l'étape de nettoyage interactif. Vous pouvez désactiver le vérificateur d'orthographe à l'étape de nettoyage interactif en cliquant sur l'icône **Activer/désactiver le vérificateur d'orthographe** , ou en cliquant avec le bouton droit dans la zone des valeurs de domaine, puis en cliquant sur **Vérificateur d'orthographe** dans le menu contextuel. Pour l'activer de nouveau, faites de même.  
  
    > [!NOTE]  
    >  La fonctionnalité de vérificateur d'orthographe est disponible uniquement dans le volet supérieur (valeurs de domaine). De plus, vous ne pouvez pas activer ou désactiver le vérificateur d'orthographe pour les domaines composites. Les domaines enfants d'un domaine composite qui sont de type chaîne, et pour lesquels la fonctionnalité de vérificateur d'orthographe est activée, ont la fonctionnalité de vérificateur d'orthographe activée par défaut à l'étape de nettoyage interactif.  
  
4.  Au cours de l'étape de nettoyage interactif, vous pouvez basculer vers le Générateur de profils en cliquant sur l'onglet **Générateur de profils** pour afficher les notifications et le profilage des données en temps réel. Pour plus d’informations, consultez [Statistiques du Générateur de profils](#Profiler).  
  
5.  Après avoir examiné toutes les valeurs de domaine, cliquez sur **Suivant** à passer à l'étape d'exportation.  
  
##  <a name="Export"></a> Étape d'exportation  
 Au cours de l'étape d'exportation, vous spécifiez des paramètres d'exportation de vos données nettoyées : ce qui doit être exporté et où.  
  
1.  Dans la page **Exporter** de l'Assistant de qualité des données pour le nettoyage, sélectionnez le type de destination pour l'exportation de vos données nettoyées : **SQL Server**, **Fichier CSV**ou **Fichier Excel**.  
  
    > [!IMPORTANT]  
    >  Si vous utilisez une version 64 bits d'Excel, vous ne pouvez pas exporter les données nettoyées vers un fichier Excel : vous ne pouvez exporter que vers une base de données SQL Server ou un fichier .csv.  
  
    1.  **SQL Server**: sélectionnez **DQS_STAGING_DATA** comme base de données de destination si vous voulez exporter vos données ici, puis indiquez le nom de la table qui sera créée pour stocker vos données exportées. Sinon, sélectionnez une autre base de données si vous voulez exporter des données vers une base de données différente, puis spécifiez le nom de la table qui sera créée pour stocker vos données exportées. Pour être disponible dans la liste déroulante [!INCLUDE[ssDQSServer](../includes/ssdqsserver-md.md)] Base de données **, votre base de données de destination doit être présente dans la même instance de SQL Server que** .  
  
    2.  **Fichier CSV**: cliquez sur **Parcourir**, puis indiquez le nom et l'emplacement du fichier .csv où vous voulez exporter les données nettoyées. Vous pouvez également taper le nom du fichier .csv avec le chemin d'accès complet où vous voulez exporter les données nettoyées. Par exemple, « c:\ExportedData.csv ». Le fichier est enregistré sur l'ordinateur où [!INCLUDE[ssDQSServer](../includes/ssdqsserver-md.md)] est installé.  
  
    3.  **Fichier Excel**: cliquez sur **Parcourir**, puis indiquez le nom et l'emplacement du fichier Excel où vous voulez exporter les données nettoyées. Vous pouvez également taper le nom du fichier Excel avec le chemin d'accès complet où vous voulez exporter les données nettoyées. Par exemple, « c:\ExportedData.xlsx ». Le fichier est enregistré sur l'ordinateur où [!INCLUDE[ssDQSServer](../includes/ssdqsserver-md.md)] est installé.  
  
2.  Activez la case à cocher **Normaliser la sortie** pour normaliser la sortie en fonction du format de sortie sélectionné pour le domaine. Par exemple, mettez la valeur de chaîne en majuscules ou mettez une majuscule à la première lettre du mot. Pour plus d'informations sur la spécification du format de sortie d'un domaine, consultez la liste **Mettre en forme la sortie vers** de la rubrique [Définir les propriétés du domaine](../data-quality-services/set-domain-properties.md).  
  
3.  Ensuite, sélectionnez la sortie de données : exportez uniquement les données nettoyées ou exportez les données nettoyées ainsi que les informations relatives au nettoyage.  
  
    -   **Données seulement**: cliquez sur la case d'option pour exporter uniquement les données nettoyées.  
  
    -   **Données et informations sur le nettoyage**: cliquez sur la case d'option pour exporter les données suivantes pour chaque domaine :  
  
        -   **\<Domaine>_Source** : valeur d’origine dans le domaine.  
  
        -   **\<Domaine>_Sortie** : valeurs nettoyées dans le domaine.  
  
        -   **\<Domaine>_Raison** : raison spécifiée pour la correction de la valeur.  
  
        -   **\<Domaine>_Confiance** : niveau de confiance pour tous les termes qui ont été corrigés. Il est affiché en tant que valeur décimale équivalente à la valeur en pourcentage correspondante. Par exemple, un niveau de confiance de 95 % est affiché sous la forme 0,9500000.  
  
        -   **\<Domaine>_État** : état de la valeur de domaine après le nettoyage des données. Par exemple, **Suggérés**, **Nouveau**, **Non valide**, **Corrigés**ou **Correct**.  
  
        -   **État de l’enregistrement** : en plus d’avoir un champ d’état pour chaque domaine mappé **(\<Nom_domaine>_État**), le champ **État de l’enregistrement** affiche l’état d’un enregistrement. Si l'état d'un des domaines dans l'enregistrement est *Nouveau* ou *Correct*, l' **État de l'enregistrement** est défini sur *Correct*. Si l'état d'un des domaines dans l'enregistrement est *Suggérés*, *Non valide*, ou *Corrigés*, l' **État de l'enregistrement** a la valeur correspondante. Par exemple, si l'état d'un des domaines dans l'enregistrement est *Suggérés*, l' **État de l'enregistrement** est défini sur *Suggérés*.  
  
            > [!NOTE]  
            >  Si vous utilisez le service de données de référence pour l'opération de nettoyage, certaines informations supplémentaires sur la valeur de domaine peuvent également être exportées. Pour plus d’informations, consultez [Nettoyer les données à l’aide de la connaissance des données de référence &#40;externes&#41;](../data-quality-services/cleanse-data-using-reference-data-external-knowledge.md).  
  
4.  Cliquez sur **Exporter** pour exporter les données vers la destination de données sélectionnée. Si vous avez sélectionné :  
  
    -   **SQL Server** comme destination des données, une nouvelle table portant le nom spécifié sera créée dans la base de données sélectionnée.  
  
    -   **Fichier CSV** comme destination de données, un fichier .csv sera créé à l'emplacement de l'ordinateur [!INCLUDE[ssDQSServer](../includes/ssdqsserver-md.md)] avec le nom de fichier que vous avez spécifié précédemment dans la zone **Nom du fichier CSV** .  
  
    -   **Fichier Excel** comme destination de données, un fichier Excel sera créé à l'emplacement de l'ordinateur [!INCLUDE[ssDQSServer](../includes/ssdqsserver-md.md)] avec le nom de fichier que vous avez spécifié précédemment dans la zone **Nom du fichier Excel** .  
  
5.  Cliquez sur **Terminer** pour fermer le projet de qualité des données.  
  
##  <a name="Profiler"></a> Profiler Statistics  
 L'onglet **Générateur de profils** fournit des statistiques qui indiquent la qualité des données sources. Le profilage vous permet d'évaluer l'efficacité de l'activité de nettoyage des données, et vous pouvez éventuellement déterminer jusqu'à quel point le nettoyage des données a permis d'améliorer la qualité des données.  
  
 L'onglet **Générateur de profils** fournit les statistiques suivantes pour les données sources, par le champ et par domaine :  
  
-   **Enregistrements**: nombre d'enregistrements, dans l'exemple de données, qui ont été analysés pour l'activité de nettoyage des données  
  
-   **Enregistrements corrects**: nombre d'enregistrements identifiés comme étant corrects  
  
-   **Enregistrements corrigés**: nombre d'enregistrements qui ont été corrigés  
  
-   **Enregistrements suggérés**: nombre d'enregistrements qui ont été suggérés  
  
-   **Enregistrements non valides**: nombre d'enregistrements qui étaient non valides  
  
 Les statistiques de champ sont les suivantes :  
  
-   **Champ**: nom du champ dans les données sources  
  
-   **Domaine**: nom du domaine mappé au champ  
  
-   **Valeurs corrigées**: nombre de valeurs de domaine qui ont été corrigées  
  
-   **Valeurs suggérées**: nombre de valeurs de domaine qui ont été suggérées  
  
-   **Exhaustivité**: exhaustivité de chaque champ source qui est mappé pour l'activité de nettoyage  
  
-   **Précision**: précision de chaque champ source qui est mappé pour l'activité de nettoyage  
  
 Le profilage DQS fournit deux dimensions de qualité des données : l' *exhaustivité* (dans quelle mesure des données sont présentes) et la *précision* (dans quelle mesure des données peuvent être utilisées pour l'usage prévu). Si le profilage vous informe qu'un champ est relativement incomplet, vous pouvez le supprimer de la base de connaissances d'un projet de qualité des données. Le profilage peut ne pas fournir des statistiques d'exhaustivité fiables pour les domaines composites. Si vous avez besoin des statistiques d'exhaustivité, utilisez des domaines simples plutôt que des domaines composites. Si vous souhaitez utiliser des domaines composites, vous pouvez créer une base de connaissances avec des domaines simples pour le profilage, afin de déterminer l'exhaustivité, et créer un autre domaine avec un domaine composite pour le processus de nettoyage. Par exemple, le profilage peut afficher une exhaustivité de 95 % pour les enregistrements d'adresse à l'aide d'un domaine composite, mais il peut y avoir un niveau bien supérieur de non-exhaustivité pour l'une des colonnes, par exemple, une colonne de code postal. Dans cet exemple, vous pouvez mesurer l'exhaustivité de la colonne de code postal avec un domaine unique. Le profilage fournira probablement des statistiques de précision fiables pour les domaines composites, car vous pouvez mesurer la précision de plusieurs colonnes ensemble. Comme la valeur de ces données se trouve dans l'agrégation composite, vous pouvez mesurer la précision avec un domaine composite.  
  
 Les statistiques de précision nécessiteront probablement plus d'interprétation si vous n'utilisez pas un service de données de référence. Si vous utilisez un service de données de référence pour le nettoyage des données, vous aurez un niveau de confiance dans les statistiques de précision. Pour plus d’informations sur le nettoyage des données à l’aide du service de données de référence, consultez [Nettoyer les données à l’aide de la connaissance des données de référence &#40;externes&#41;](../data-quality-services/cleanse-data-using-reference-data-external-knowledge.md).  
  
### <a name="cleansing-notifications"></a>Notifications de nettoyage  
 Les conditions suivantes génèrent des notifications :  
  
-   Il n'y a aucune correction ou suggestion pour un champ. Vous pouvez le supprimer du mappage, exécuter d'abord une découverte des connaissances ou utiliser une autre base de connaissances.  
  
-   Il y a relativement peu de corrections ou suggestions pour un champ. Vous pouvez le supprimer du mappage, exécuter d'abord une découverte des connaissances ou utiliser une autre base de connaissances.  
  
-   Le niveau de précision du champ est très faible. Vous pouvez vérifier le mappage, ou envisager d'exécuter d'abord une découverte des connaissances.  
  
 Pour plus d'informations sur le profilage, consultez [Profilage des données et notifications dans DQS](../data-quality-services/data-profiling-and-notifications-in-dqs.md).  
  
  
