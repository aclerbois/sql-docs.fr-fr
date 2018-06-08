---
title: Source ODBC | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: data-flow
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.ssis.designer.odbcsource.f1
- sql13.ssis.designer.odbcsource.connection.f1
- sql13.ssis.designer.odbcsource.columns.f1
- sql13.ssis.designer.odbcsource.errorhandling.f1
ms.assetid: abcf34eb-9140-4100-82e6-b85bccd22abe
caps.latest.revision: 9
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: c67cbfeb3797c2e0d9fb5758078dad96f290d4e7
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="odbc-source"></a>Source ODBC
  La source ODBC extrait les données d'une base de données compatible ODBC à l'aide d'une table de base de données, d'une vue ou d'une instruction SQL.  
  
 La source ODBC utilise les modes d'accès aux données suivants pour extraire les données :  
  
-   Une table ou une vue.  
  
-   Les résultats d'une instruction SQL.  
  
 La source utilise un gestionnaire de connexions ODBC qui spécifie le fournisseur à utiliser.  
  
 Une source ODBC inclut les colonnes de sortie de données sources. Lorsque les colonnes de sortie sont mappées dans la destination ODBC aux colonnes de destination, des erreurs peuvent se produire si aucune colonne de sortie n'est mappée aux colonnes de destination. Même si des colonnes de types différents peuvent être mappées, si les données de sortie ne sont pas compatibles pour la destination, une erreur se produit au moment de l'exécution. En fonction du comportement des erreurs, l'erreur sera ignorée, entraînera un échec ou la ligne sera envoyée à la sortie d'erreur.  
  
 La source ODBC a une sortie normale et une sortie d'erreur.  
  
## <a name="error-handling"></a>Gestion des erreurs  
 La source ODBC a une sortie d'erreur. La sortie d'erreur du composant contient les colonnes de sortie suivantes :  
  
-   **Code d’erreur**: numéro qui correspond à l’erreur actuelle. Consultez la documentation de la base de données ODBC que vous utilisez pour obtenir une liste d'erreurs. Pour obtenir la liste des codes d'erreur SSIS, consultez le Guide de référence des erreurs et des événements SSIS.  
  
-   **Colonne d’erreur**: colonne source à l’origine de l’erreur (pour les erreurs de conversion).  
  
-   Colonnes de données de sortie standard.  
  
 Selon le comportement paramétré pour les erreurs, la source ODBC prend en charge les erreurs de retour (conversion de données, troncation) qui se produisent pendant le processus d'extraction dans la sortie d'erreur. Pour plus d’informations, consultez [Éditeur de destination ODBC &#40;page Gestionnaire de connexions&#41;](../../integration-services/data-flow/odbc-destination-editor-connection-manager-page.md).  
  
## <a name="data-type-support"></a>Prise en charge du type de données  
 Pour plus d’informations sur les types de données pris en charge par la source ODBC, consultez le Connecteur pour Open Database Connectivity (ODBC).  
  
## <a name="extract-options"></a>Options d'extraction  
 La source ODBC s’exécute en mode **Lot** ou **Ligne par ligne** . Le mode utilisé est déterminé par la propriété **FetchMethod** . La liste suivante décrit les différents modes.  
  
-   **Lot**: les composants tentent d’utiliser la méthode de récupération la plus efficace en fonction des capacités perçues du fournisseur ODBC. Pour la plupart des fournisseurs ODBC modernes, il s’agit de SQLFetchScroll avec une liaison de table (où la taille de la table est déterminée par la propriété **BatchSize** ). Si vous sélectionnez **Lot** et que le fournisseur ne prend pas en charge cette méthode, la destination ODBC bascule automatiquement en mode **Ligne par ligne** .  
  
-   **Ligne par ligne**: le composant utilise SQLFetch pour extraire les lignes une par une.  
  
 Pour plus d’informations sur la propriété **FetchMethod** , consultez [Propriétés personnalisées des sources ODBC](../../integration-services/data-flow/odbc-source-custom-properties.md).  
  
## <a name="parallelism"></a>Parallelism  
 Il n'existe aucune limitation quant au nombre de composants de source ODBC pouvant s'exécuter en parallèle sur la même table ou des tables différentes, sur le même ordinateur ou sur des ordinateurs différents (autre que les limites de session globale habituelles).  
  
 Toutefois, les limites du fournisseur ODBC utilisé peuvent limiter le nombre de connexions simultanées par le fournisseur. Ces limites restreignent le nombre d'instances parallèles prises en charge pour la source ODBC. Le développeur SSIS doit avoir connaissance des limites de tout fournisseur ODBC utilisé et en tenir compte lors de la génération de packages SSIS.  
  
## <a name="troubleshooting-the-odbc-source"></a>Résolution des problèmes liés à la source ODBC  
 Vous pouvez consigner dans un journal les appels que la source ODBC effectue vers des fournisseurs de données externes. Cette fonctionnalité de journalisation permet de résoudre des problèmes liés au chargement de données qu'effectue la source ODBC à partir de sources de données externes. Pour consigner les appels effectués par la source ODBC à destination de fournisseurs de données externes, activez la trace du gestionnaire de pilotes ODBC. Pour plus d’informations, consultez la documentation Microsoft intitulée *Génération d’une trace ODBC avec l’administrateur des source de données ODBC*.  
  
## <a name="configuring-the-odbc-source"></a>Configuration de la source ODBC  
 Vous pouvez définir la source ODBC par programmation ou par le biais du concepteur SSIS.  
  
 La boîte de dialogue **Éditeur avancé** contient les propriétés qui peuvent être définies par programme.  
  
 Pour ouvrir la boîte de dialogue **Éditeur avancé** :  
  
-   Sur l’écran **Flux de données** de votre projet [!INCLUDE[ssISCurrent](../../includes/ssiscurrent-md.md)] , cliquez avec le bouton droit sur la source ODBC, puis sélectionnez **Afficher l’éditeur avancé**.  
  
 Pour plus d’informations sur les propriétés que vous pouvez définir dans la boîte de dialogue Éditeur avancé, consultez [Propriétés personnalisées des sources ODBC](../../integration-services/data-flow/odbc-source-custom-properties.md).  
  
## <a name="in-this-section"></a>Dans cette section  
  
-   [Extraire des données à l’aide de la source ODBC](../../integration-services/data-flow/extract-data-by-using-the-odbc-source.md)  
  
-   [Propriétés personnalisées des sources ODBC](../../integration-services/data-flow/odbc-source-custom-properties.md)  
  
## <a name="odbc-source-editor-connection-manager-page"></a>Éditeur de source ODBC (page Gestionnaire de connexions)
  Utilisez la page **Gestionnaire de connexions** de la boîte de dialogue **Éditeur de source ODBC** pour sélectionner le gestionnaire de connexions ODBC pour la source. Cette page vous permet également de sélectionner une table ou une vue à partir de la base de données.  
  
### <a name="task-list"></a>Liste des tâches  
 **Pour ouvrir l'Éditeur de source ODBC (page Gestionnaire de connexions)**  
  
-   Dans [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)], ouvrez le package [!INCLUDE[ssISCurrent](../../includes/ssiscurrent-md.md)] qui possède la source ODBC.  
  
-   Sous l’onglet **Flux de données** , double-cliquez sur la source ODBC.  
  
### <a name="options"></a>Options  
  
#### <a name="connection-manager"></a>Gestionnaire de connexions  
 Sélectionnez un gestionnaire de connexions ODBC existant dans la liste ou cliquez sur **Nouveau** pour créer une nouvelle connexion. La connexion peut concerner n'importe quelle base de données prise en charge par ODBC.  
  
#### <a name="new"></a>Nouvelle  
 Cliquez sur **Nouveau**. La boîte de dialogue **Configurer l'Éditeur du gestionnaire de connexions ODBC** s'ouvre et vous permet de créer un nouveau gestionnaire de connexions ODBC.  
  
#### <a name="data-access-mode"></a>Mode d'accès aux données  
 Spécifiez la méthode de sélection des données dans la source. Ces fonctions sont répertoriées dans le tableau suivant :  
  
|Option|Description|  
|------------|-----------------|  
|Nom de la table|Permet de récupérer les données d'une table ou d'une vue dans la source de données ODBC. Lorsque vous sélectionnez cette option, sélectionnez une valeur parmi les suivantes dans la liste :|  
||**Nom de la table ou de la vue**: sélectionnez une table ou une vue disponible dans la liste ou tapez une expression régulière pour identifier la table.|  
||Cette liste contient les 1 000 premières tables uniquement. Si votre base de données contient plus de 1 000 tables, vous pouvez taper le début du nom d'une table ou utiliser le caractère générique (*) pour entrer une partie du nom afin d'afficher la table ou les tables que vous souhaitez utiliser.|  
|Commande SQL|Extrayez les données de la source de données ODBC à l'aide d'une requête SQL. Vous devez écrire la requête dans la syntaxe de la base de données source dans laquelle vous travaillez. Lorsque vous sélectionnez cette option, entrez une requête selon l'une des méthodes suivantes :|  
||Entrez le texte de la requête SQL dans le champ **Texte de la commande SQL** .|  
||Cliquez sur **Parcourir** pour charger la requête SQL à partir d'un fichier texte.|  
||Pour vérifier la syntaxe du texte de la requête, cliquez sur **Analyser** .|  
  
#### <a name="preview"></a>Aperçu  
 Cliquez sur **Aperçu** pour afficher les 200 premières lignes de données extraites de la table ou de la vue sélectionnée.  
  
## <a name="odbc-source-editor-columns-page"></a>Éditeur de source ODBC (page Colonnes)
  Utilisez la page **Colonnes** de la boîte de dialogue **Éditeur de source ODBC** pour mapper une colonne de sortie à chaque colonne externe (source).  
  
### <a name="task-list"></a>Liste des tâches  
 **Pour ouvrir l'Éditeur de source ODBC (page Colonnes)**  
  
1.  Dans [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)], ouvrez le package [!INCLUDE[ssISCurrent](../../includes/ssiscurrent-md.md)] qui possède la source ODBC.  
  
2.  Sous l’onglet **Flux de données** , double-cliquez sur la source ODBC.  
  
3.  Dans l' **Éditeur de source ODBC**, cliquez sur **Colonnes**.  
  
### <a name="options"></a>Options  
  
#### <a name="available-external-columns"></a>Colonnes externes disponibles  
 Liste des colonnes externes disponibles dans la source de données. Vous ne pouvez pas ajouter ou supprimer des colonnes à l'aide de cette table. Sélectionnez les colonnes à utiliser dans la source. Les colonnes sélectionnées sont ajoutées à la liste **Colonne externe** dans l'ordre de leur sélection.  
  
 Activez la case à cocher **Sélectionner tout** pour sélectionner toutes les colonnes.  
  
#### <a name="external-column"></a>Colonne externe  
 Vue des colonnes externes (sources) selon l'ordre dans lequel vous les visualisez lorsque vous configurez des composants qui consomment des données à partir de la source ODBC.  
  
#### <a name="output-column"></a>Colonne de sortie  
 Spécifiez un nom unique pour chaque colonne de sortie. Le nom par défaut est celui de la colonne externe (source) sélectionnée ; vous pouvez néanmoins choisir n'importe quel nom unique et significatif. Le nom entré est affiché dans le concepteur SSIS.  
  
## <a name="odbc-source-editor-error-output-page"></a>Éditeur de source ODBC (page Sortie d'erreur)
  Utilisez la page **Sortie d'erreur** de la boîte de dialogue **Éditeur de source ODBC** pour sélectionner les options de gestion des erreurs.  
  
### <a name="task-list"></a>Liste des tâches  
 **Pour ouvrir l'Éditeur de source ODBC (page Sortie d'erreur)**  
  
-   Dans [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)], ouvrez le package [!INCLUDE[ssISCurrent](../../includes/ssiscurrent-md.md)] qui possède la source ODBC.  
  
-   Sous l’onglet **Flux de données** , double-cliquez sur la source ODBC.  
  
-   Dans l' **Éditeur de source ODBC**, cliquez sur **Sortie d'erreur**.  
  
### <a name="options"></a>Options  
  
#### <a name="inputoutput"></a>Entrée/sortie  
 Affichez le nom de la source de données.  
  
#### <a name="column"></a>colonne  
 Non utilisé.  
  
#### <a name="error"></a>Error  
 Sélectionnez la façon dont la source ODBC doit gérer les erreurs dans un flux : ignorer l'échec, rediriger la ligne ou faire échouer le composant.  
  
#### <a name="truncation"></a>Troncation  
 Sélectionnez la façon dont la source ODBC doit gérer la troncation dans un flux : ignorer l'échec, rediriger la ligne ou faire échouer le composant.  
  
#### <a name="description"></a>Description  
 Non utilisé.  
  
#### <a name="set-this-value-to-selected-cells"></a>Définir cette valeur sur les cellules sélectionnées  
 Sélectionnez la façon dont la source ODBC gère l'ensemble des cellules sélectionnées lorsqu'une erreur ou une troncation se produit : ignorer l'échec, rediriger la ligne ou faire échouer le composant.  
  
#### <a name="apply"></a>Appliquer  
 Appliquez les options de gestion des erreurs aux cellules sélectionnées.  
  
### <a name="error-handling-options"></a>Options de gestion des erreurs  
 Vous pouvez utiliser les options suivantes pour configurer la façon dont la source ODBC gère les erreurs et les troncations.  
  
#### <a name="fail-component"></a>Composant défaillant  
 La tâche de flux de données échoue lorsqu'une erreur ou une troncation a lieu. Il s'agit du comportement par défaut.  
  
#### <a name="ignore-failure"></a>Ignorer l'échec  
 L'erreur ou la troncation est ignorée.  
  
#### <a name="redirect-flow"></a>Rediriger le flux  
 La ligne qui provoque l'erreur ou la troncation est dirigée vers la sortie d'erreur de la source ODBC.  
  
  
