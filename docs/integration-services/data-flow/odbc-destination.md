---
title: Destination ODBC | Microsoft Docs
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
- sql13.ssis.designer.odbcdest.f1
- sql13.ssis.designer.odbcdest.connection.f1
- sql13.ssis.designer.odbcdest.columns.f1
- sql13.ssis.designer.odbcdest.errorhandling.f1
ms.assetid: bffa63e0-c737-4b54-b4ea-495a400ffcf8
caps.latest.revision: 12
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: aeaf5354106c548a8c0b107a99a9b0778be8006d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="odbc-destination"></a>Destination ODBC
  La destination ODBC charge en masse les données dans les tables de base de données compatibles ODBC. La destination ODBC utilise un gestionnaire de connexions ODBC pour se connecter à la source de données.  
  
 Une destination ODBC inclut des mappages entre les colonnes d'entrée et les colonnes de la source de données de destination. Vous n’avez pas besoin de mapper les colonnes d’entrée à toutes les colonnes de destination, mais en fonction des propriétés des colonnes de destination, des erreurs peuvent se produire si aucune colonne d’entrée n’est mappée aux colonnes de destination. Par exemple, si une colonne de destination n'autorise pas les valeurs null, une colonne d'entrée doit être mappée à cette colonne. En outre, même si des colonnes de types différents peuvent être mappées, si les données d'entrée ne sont pas compatibles pour le type de colonne de destination, une erreur se produit au moment de l'exécution. En fonction du comportement des erreurs configuré, l'erreur sera ignorée, entraînera un échec ou la ligne sera envoyée à la sortie d'erreur.  
  
 La destination ODBC comporte une sortie standard et une sortie d'erreur.  
  
##  <a name="BKMK_odbcdestination_loadoptions"></a> Options de chargement  
 La destination ODBC peut utiliser l'un des deux modules de charge d'accès. Vous définissez le mode dans l’[Éditeur de source ODBC &#40;page Gestionnaire de connexions&#41;](../../integration-services/data-flow/odbc-source-editor-connection-manager-page.md). Les deux modes sont :  
  
-   **Lot**: dans ce mode, la destination ODBC tente d’utiliser la méthode d’insertion la plus efficace en fonction des capacités perçues du fournisseur ODBC. Pour la plupart des fournisseurs modernes ODBC, cela implique de préparer une instruction INSERT avec des paramètres, puis d’utiliser une liaison de paramètre de table selon les lignes (où la taille de la table est contrôlée par la propriété **BatchSize** ). Si vous sélectionnez **Lot** et que le fournisseur ne prend pas en charge cette méthode, la destination ODBC bascule automatiquement en mode **Ligne par ligne** .  
  
-   **Ligne par ligne**: dans ce mode, la destination ODBC prépare une instruction INSERT avec des paramètres et utilise **SQL Execute** pour insérer les lignes une par une.  
  
## <a name="error-handling"></a>Gestion des erreurs  
 La destination ODBC a une sortie d'erreur. La sortie d'erreur du composant contient les colonnes de sortie suivantes :  
  
-   **Code d’erreur**: numéro qui correspond à l’erreur actuelle. Consultez la documentation de votre base de données source pour obtenir la liste des erreurs. Pour obtenir la liste des codes d'erreur SSIS, consultez le Guide de référence des erreurs et des événements SSIS.  
  
-   **Colonne d’erreur**: colonne source à l’origine de l’erreur (pour les erreurs de conversion).  
  
-   Colonnes de données de sortie standard.  
  
 Selon le comportement paramétré pour les erreurs, la destination ODBC prend en charge les erreurs de retour (conversion de données, troncation) qui se produisent pendant le processus de récupération dans la sortie d'erreur. Pour plus d’informations, consultez [Éditeur de source ODBC &#40;page Sortie d’erreur&#41;](../../integration-services/data-flow/odbc-source-editor-error-output-page.md).  
  
## <a name="parallelism"></a>Parallélisme  
 Il n'existe aucune limitation quant au nombre de composants de destination ODBC pouvant s'exécuter en parallèle sur la même table ou des tables différentes, sur le même ordinateur ou sur des ordinateurs différents (autre que les limites de session globale habituelles).  
  
 Toutefois, les limites du fournisseur ODBC utilisé peuvent limiter le nombre de connexions simultanées par le fournisseur. Ces limites restreignent le nombre d'instances parallèles prises en charge pour la destination ODBC. Le développeur SSIS doit avoir connaissance des limites de tout fournisseur ODBC utilisé et en tenir compte lors de la génération de packages SSIS.  
  
 Vous devez également savoir qu'un chargement simultané dans la même table peut réduire les performances en raison du verrouillage des enregistrements standard. Cela dépend des données chargées et de l'organisation de table.  
  
## <a name="troubleshooting-the-odbc-destination"></a>Résolution des problèmes liés à la destination ODBC  
 Vous pouvez consigner dans un journal les appels que la source ODBC effectue vers des fournisseurs de données externes. Cette fonctionnalité de journalisation permet de résoudre des problèmes liés à l'enregistrement de données vers des sources de données externes que réalise la destination ODBC. Pour consigner les appels effectués par la destination ODBC vers des fournisseurs ODBC externes, activez la trace du gestionnaire de pilotes ODBC. Pour plus d’informations, consultez la documentation Microsoft sur la *génération d’une trace ODBC avec l’administrateur de sources de données ODBC*.  
  
## <a name="configuring-the-odbc-destination"></a>Configuration de la destination ODBC  
 Vous pouvez configurer la destination ODBC par programmation ou par le biais du concepteur SSIS  
  
 Pour plus d’informations, consultez l’une des rubriques suivantes :  
  
-   [Éditeur de destination ODBC &#40;page Gestionnaire de connexions&#41;](../../integration-services/data-flow/odbc-destination-editor-connection-manager-page.md)  
  
-   [Éditeur de destination ODBC &#40;page Mappages&#41;](../../integration-services/data-flow/odbc-destination-editor-mappings-page.md)  
  
-   [Éditeur de destination ODBC &#40;page Sortie d’erreur&#41;](../../integration-services/data-flow/odbc-destination-editor-error-output-page.md)  
  
 La boîte de dialogue **Éditeur avancé** contient les propriétés qui peuvent être définies par programme.  
  
 Pour ouvrir la boîte de dialogue **Éditeur avancé** :  
  
-   Sur l’écran **Flux de données** de votre projet [!INCLUDE[ssISCurrent](../../includes/ssiscurrent-md.md)] , cliquez avec le bouton droit sur la destination ODBC, puis sélectionnez **Afficher l’éditeur avancé**.  
  
 Pour plus d’informations sur les propriétés que vous pouvez définir dans la boîte de dialogue Éditeur avancé, consultez [Propriétés personnalisées des destinations ODBC](../../integration-services/data-flow/odbc-destination-custom-properties.md).  
  
## <a name="in-this-section"></a>Dans cette section  
  
-   [Charger des données à l’aide de la destination ODBC](../../integration-services/data-flow/load-data-by-using-the-odbc-destination.md)  
  
-   [Propriétés personnalisées des destinations ODBC](../../integration-services/data-flow/odbc-destination-custom-properties.md)  
  
## <a name="odbc-destination-editor-connection-manager-page"></a>Éditeur de destination ODBC (page Gestionnaire de connexions)
  Utilisez la page **Gestionnaire de connexions** de la boîte de dialogue **Éditeur de destination ODBC** pour sélectionner le gestionnaire de connexions ODBC de la destination. Cette page vous permet également de sélectionner une table ou une vue à partir de la base de données.  
  
 **Pour ouvrir l'Éditeur de destination ODBC (page Gestionnaire de connexions)**  
  
### <a name="task-list"></a>Liste des tâches  
  
-   Dans [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)], ouvrez le package [!INCLUDE[ssISCurrent](../../includes/ssiscurrent-md.md)] qui possède la destination ODBC.  
  
-   Sous l’onglet **Flux de données** , double-cliquez sur la destination ODBC.  
  
-   Dans l' **Éditeur de destination ODBC**, cliquez sur **Gestionnaire de connexions**.  
  
### <a name="options"></a>Options  
  
#### <a name="connection-manager"></a>Gestionnaire de connexions  
 Sélectionnez un gestionnaire de connexions ODBC existant dans la liste ou cliquez sur Nouveau pour créer une nouvelle connexion. La connexion peut concerner n'importe quelle base de données prise en charge par ODBC.  
  
#### <a name="new"></a>Nouvelle  
 Cliquez sur **Nouveau**. La boîte de dialogue **Configurer l'Éditeur du gestionnaire de connexions ODBC** s'ouvre et vous permet de créer un nouveau gestionnaire de connexions.  
  
#### <a name="data-access-mode"></a>Mode d'accès aux données  
 Spécifiez la méthode de chargement des données dans la destination. Ces fonctions sont répertoriées dans le tableau suivant :  
  
|Option|Description|  
|------------|-----------------|  
|Nom de la table - Lot|Sélectionnez cette option pour configurer la destination ODBC en mode par lot. Lorsque vous sélectionnez cette option, les options suivantes sont disponibles :|  
||**Nom de la table ou de la vue**: sélectionnez une table ou une vue disponible dans la liste.<br /><br /> Cette liste contient les 1 000 premières tables uniquement. Si votre base de données contient plus de 1000 tables, vous pouvez taper le début du nom d’une table ou utiliser le caractère générique (\*) pour entrer une partie du nom afin d’afficher la table ou les tables que vous souhaitez utiliser.<br /><br /> **Taille du lot**: entrez la taille du lot pour le chargement en bloc. Il s'agit du nombre de lignes chargées dans un même lot.|  
|Nom de la table - Ligne par ligne|Sélectionnez cette option pour configurer la destination ODBC de manière à insérer les lignes dans la table de destination une par une. Lorsque vous sélectionnez cette option, l'option suivante est disponible :|  
||**Nom de la table ou de la vue**: sélectionnez dans la liste une table ou une vue disponible dans la base de données.<br /><br /> Cette liste contient les 1 000 premières tables uniquement. Si votre base de données contient plus de 1 000 tables, vous pouvez taper le début du nom d'une table ou utiliser le caractère générique (*) pour entrer une partie du nom afin d'afficher la table ou les tables que vous souhaitez utiliser.|  
  
#### <a name="preview"></a>Aperçu  
 Cliquez sur **Aperçu** pour afficher jusqu'à 200 lignes de données pour la table sélectionnée.  
  
## <a name="odbc-destination-editor-mappings-page"></a>Éditeur de destination ODBC (page Mappages)
  La page **Mappages** de la boîte de dialogue **Éditeur de destination ODBC** vous permet de mapper les colonnes d’entrée aux colonnes de destination.  
  
### <a name="options"></a>Options  
  
#### <a name="available-input-columns"></a>Colonnes d’entrée disponibles  
 Liste des colonnes d'entrée disponibles. Par glisser-déplacer, mappez une colonne d'entrée à une colonne de destination disponible.  
  
#### <a name="available-destination-columns"></a>Colonnes de destination disponibles  
 Liste des colonnes de destination disponibles. Par glisser-déplacer, mappez une colonne de destination à une colonne d'entrée disponible.  
  
#### <a name="input-column"></a>Colonne d'entrée  
 Affichez les colonnes d’entrée que vous avez sélectionnées. Vous pouvez supprimer des mappages en sélectionnant **\<ignorer>** de manière à exclure des colonnes de la sortie.  
  
#### <a name="destination-column"></a>Colonne de destination  
 Affiche toutes les colonnes de destination disponibles, mappées et non mappées.  
  
## <a name="odbc-destination-editor-error-output-page"></a>Éditeur de destination ODBC (page Sortie d'erreur)
  Utilisez la page **Sortie d'erreur** de la boîte de dialogue **Éditeur de destination ODBC** pour sélectionner les options de gestion des erreurs.  
  
 **Pour ouvrir l'Éditeur de destination ODBC (page Sortie d'erreur)**  
  
### <a name="task-list"></a>Liste des tâches  
  
-   Dans [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)], ouvrez le package [!INCLUDE[ssISCurrent](../../includes/ssiscurrent-md.md)] qui possède la destination ODBC.  
  
-   Sous l’onglet **Flux de données** , double-cliquez sur la destination ODBC.  
  
-   Dans l' **Éditeur de destination ODBC**, cliquez sur **Sortie d'erreur**.  
  
### <a name="options"></a>Options  
  
#### <a name="inputoutput"></a>Entrée/sortie  
 Affichez le nom de la source de données.  
  
#### <a name="column"></a>colonne  
 Non utilisé.  
  
#### <a name="error"></a>Error  
 Sélectionnez la façon dont la destination ODBC doit gérer les erreurs dans un flux : ignorer l'échec, rediriger la ligne ou faire échouer le composant.  
  
#### <a name="truncation"></a>Troncation  
 Sélectionnez la façon dont la destination ODBC doit gérer la troncation dans un flux : ignorer l'échec, rediriger la ligne ou faire échouer le composant.  
  
#### <a name="description"></a>Description  
 Affichez la description d'une erreur.  
  
#### <a name="set-this-value-to-selected-cells"></a>Définir cette valeur sur les cellules sélectionnées  
 Sélectionnez la façon dont la destination ODBC gère l'ensemble des cellules sélectionnées lorsqu'une erreur ou une troncation se produit : ignorer l'échec, rediriger la ligne ou faire échouer le composant.  
  
#### <a name="apply"></a>Appliquer  
 Appliquez les options de gestion des erreurs aux cellules sélectionnées.  
  
### <a name="error-handling-options"></a>Options de gestion des erreurs  
 Vous pouvez utiliser les options suivantes pour configurer la façon dont la destination ODBC gère les erreurs et les troncations.  
  
#### <a name="fail-component"></a>Composant défaillant  
 La tâche de flux de données échoue lorsqu'une erreur ou une troncation a lieu. Il s'agit du comportement par défaut.  
  
#### <a name="ignore-failure"></a>Ignorer l'échec  
 L'erreur ou la troncation est ignorée.  
  
#### <a name="redirect-flow"></a>Rediriger le flux  
 La ligne qui provoque l'erreur ou la troncation est dirigée vers la sortie d'erreur de la destination ODBC. Pour plus d'informations, consultez Destination ODBC.  
  
