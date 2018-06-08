---
title: Projets connexes pour les Solutions d’exploration de données | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: data-mining
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: f884a7d70447771769ba2d6ff8928234095c7cae
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="related-projects-for-data-mining-solutions"></a>Projets connexes pour des solutions d'exploration de données
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Une solution d'exploration de données requiert au minimum le projet d'exploration de données, lequel définit les sources de données, les vues de source de données, ainsi que les structures et modèles d'exploration de données. Toutefois, lorsque les modèles d'exploration de données sont utilisés dans les prises de décision quotidiennes, il est important que l'exploration de données soit intégrée à l'autre partie d'une solution d'analyse prédictive, qui peut inclure ces processus et composants :  
  
-   Préparation et sélection des données et des variables. Inclut le nettoyage de données, la gestion et l'intégration des métadonnées de plusieurs sources de données, ainsi que la conversion, la fusion et le téléchargement de données dans un entrepôt de données.  
  
-   Rapports d'analyse, présentation des prédictions et audit/suivi des activités d'exploration de données.  
  
-   Utilisation des modèles multidimensionnels ou des modèles tabulaires pour explorer les résultats.  
  
-   Amélioration de la solution d'exploration de données pour prendre en charge des nouvelles données ou les modifications de l'infrastructure de support contrôlées par l'analyse actuelle.  
  
 Cette rubrique décrit les autres fonctionnalités d' [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] qui font souvent partie d'une solution d'analyse prédictive, soit pour prendre en charge les processus de préparation et d'exploration des données, soit pour prendre en charge les utilisateurs en fournissant les outils pour l'analyse et l'action.  
  
 [Integration Services](#bkmk_SSIS)  
  
 [Reporting Services](#bkmk_SSRS)  
  
 [Data Quality Service](#bkmk_DQSetc)  
  
 [Recherche en texte intégral](#bkmk_FTSetc)  
  
 [Indexation sémantique](#bkmk_SemSearch)  
  
##  <a name="bkmk_SSIS"></a> SQL Server Integration Services  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]Fournit des composants et fonctionnalités qui sont nécessaires pour les phases de formation d’un projet d’exploration de données et de préparation des données. Bien que vous puissiez effectuer de nombreuses tâches de nettoyage et de préparation des données à l'aide d'autres outils, tels que les scripts, [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] présente beaucoup d'avantages pour l'exploration de données :  
  
-   Représente des tâches dans le cadre d'un flux de travail, qui peuvent être répétées, automatisées, ramifiées et étendues.  
  
-   Fournit une prise en charge complète de l'audit, ainsi que plusieurs méthodes pour capturer les erreurs et les événements de journalisation.  
  
     En plus de capturer le lignage des données, vous pouvez contrôler les modifications apportées aux données tout le long du pipeline de transformation des données.  
  
     Vous pouvez également intégrer vos flux de travail SSIS aux fonctionnalités prenant en charge la fonction de capture de données modifiées dans SQL Server.  
  
-   L'exploration de données peut être incorporée au flux de travail d' [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] pour répartir intelligemment les données entrantes dans plusieurs tables. Par exemple, vous pouvez utiliser une requête de prédiction pour répartir vos nouveaux clients dans différents groupes afin de pouvoir cibler lors d'une campagne de publipostage.  
  
 Les listes suivantes fournissent des liens vers les composants d' [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] qui sont les plus largement utilisés pour prendre en charge l'exploration de données.  
  
 **Composants de flux de contrôle**  
  
-   [Tâche DDL d’exécution d’Analysis Services](../../integration-services/control-flow/analysis-services-execute-ddl-task.md)  
  
-   [Tâche de traitement Analysis Services](../../integration-services/control-flow/analysis-services-processing-task.md)  
  
-   [Tâche de contrôle de capture de données modifiées](../../integration-services/control-flow/cdc-control-task.md)  
  
-   [Nettoyage de données](../../data-quality-services/data-cleansing.md)  
  
-   [Tâche de requête d’exploration de données](../../integration-services/control-flow/data-mining-query-task.md)  
  
-   [Tâche de profilage des données](../../integration-services/control-flow/data-profiling-task.md)  
  
 **Composants de flux de données**  
  
-   [Composants de flux de capture de données modifiées](../../integration-services/data-flow/cdc-flow-components.md)  
  
-   [Transformation de fractionnement conditionnel](../../integration-services/data-flow/transformations/conditional-split-transformation.md)  
  
-   [Transformation de Conversion de données](../../integration-services/data-flow/transformations/data-conversion-transformation.md)  
  
-   [Données Destination de formation du modèle d’exploration de données](../../integration-services/data-flow/data-mining-model-training-destination.md)  
  
-   [Transformation de requête d’exploration de données](../../integration-services/data-flow/transformations/data-mining-query-transformation.md)  
  
-   [Transformation de colonne dérivée](../../integration-services/data-flow/transformations/derived-column-transformation.md)  
  
-   [Transformation d’échantillonnage par pourcentage](../../integration-services/data-flow/transformations/percentage-sampling-transformation.md)  
  
-   [Transformation d’Extraction de terme](../../integration-services/data-flow/transformations/term-extraction-transformation.md)  
  
-   [Transformation de recherche de terme](../../integration-services/data-flow/transformations/term-lookup-transformation.md)  
  
##  <a name="bkmk_SSRS"></a> SQL Server Reporting Services  
 Bien que [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] ne soit généralement pas perçu comme un composant incontournable des solutions d'exploration de données, il fournit les fonctionnalités suivantes qui sont utiles pour la présentation des solutions d'exploration de données.  
  
-   Intégration des données de plusieurs sources dans des rapports complexes. Créez des requêtes sur le contenu du modèle pour les analystes et créez des rapports qui présentent des prédictions et des tendances pour les utilisateurs finaux.  
  
-   La capacité de créer un rapport permettant aux utilisateurs d'effectuer directement des requêtes sur un modèle d'exploration de données existant.  
  
-   Intégration à [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]pour prendre en charge l'extraction et l'exploration de dimensions d'exploration de données et de cubes d'exploration de données créés à partir de modèles OLAP.  
  
-   Fonctionnalités de paramétrage et de mise en forme disponibles dans [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].  
  
 Pour plus d'informations sur l'utilisation de Reporting Services avec les requêtes DMX comme source de données, consultez les liens suivants :  
  
 [Récupérer des données à partir d’un modèle d’exploration de données & #40 ; DMX & #41 ; & #40 ; SSRS & #41 ;](../../reporting-services/report-data/retrieve-data-from-a-data-mining-model-dmx-ssrs.md)  
  
 [Interface utilisateur du Concepteur de requêtes Analysis Services DMX](../../reporting-services/report-data/analysis-services-dmx-query-designer-user-interface.md)  
  
 [Type de connexion Analysis Services pour DMX & #40 ; SSRS & #41 ;](../../reporting-services/report-data/analysis-services-connection-type-for-dmx-ssrs.md)  
  
 Toutefois, il n'est pas nécessaire d'utiliser DMX comme source de données. Les composants d' [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] pour l'exploration de données prennent également en charge l'enregistrement des résultats d'une requête de prédiction dans une base de données relationnelle. Si vous avez établi un flux de travail pour mettre à jour des modèles à l'aide d' [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], les prédictions persistantes et d'autres résultats de requête d'exploration de données dans SQL Server vous permettent d'utiliser [!INCLUDE[ssCrescent](../../includes/sscrescent-md.md)] pour créer des rapports, ainsi que d'autres outils qui n'ont pas d'interface avec DMX.  
  
 Pour plus d'informations sur l'utilisation de Reporting Services comme couche de présentation pour les sources de données, consultez [Integrating Reporting Services into Applications](../../reporting-services/application-integration/integrating-reporting-services-into-applications.md).  
  
##  <a name="bkmk_DQSetc"></a> Data Quality Services  
 Data Quality Services (DQS) est une nouveauté de [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]. L’exploration des données est parfois impossible à cause de problèmes liés aux données. Pour les utilisateurs des fonctionnalités d’exploration de données qui effectuent des analyses régulières ou qui travaillent dans de grandes entreprises avec des sources de données complexes, un projet de données bien planifié à l’aide de DQS sera une solution plus fiable pour la prise en charge de l’exploration de données que le nettoyage ad hoc des données à l’aide de [!INCLUDE[tsql](../../includes/tsql-md.md)] ou d’autres scripts.  
  
 Les fonctionnalités suivantes de DQS doivent être prises en compte pour la préparation et l'intégrité des données dans une solution d'exploration de données.  
  
 **Un processus de nettoyage de données assisté par ordinateur qui analyse les données sources et suggère des modifications.**  
 DQS peut comparer des données sources avec des données de référence basées dans le Cloud conservées et garanties par les fournisseurs de qualité des données.  
  
 DQS peut également analyser les données sources brutes et créer une base de connaissances avec les données utilisateur. Les données traitées sont classées puis affichées à l'utilisateur pour d'autres traitements. Le processus de nettoyage est interactif, ce qui signifie que le gestionnaire de données peut approuver, rejeter ou modifier les données proposées par le processus de nettoyage des données assisté par ordinateur.  
  
 Le résultat du processus est une base de connaissances que vous pouvez améliorer en permanence, ou réutiliser dans plusieurs phases d'amélioration des données.  
  
 Pour plus d'informations, voir [Data Cleansing](../../data-quality-services/data-cleansing.md).  
  
 **Un processus de correspondance de données assisté par ordinateur qui analyse les données sources et suggère des modifications.**  
 Pour éviter la duplication de données, vous pouvez effectuer d'autres nettoyages de la source de données afin d'identifier les correspondances exactes et approximatives. Ces composants vous permettent de spécifier des règles de correspondance, ainsi que les seuils auxquels les appliquer.  
  
 Une fois que vous avez trouvé des correspondances de données, vous pouvez supprimer les doublons car ils peuvent représenter un problème pour l'exploration de données. La déduplication de données n'est pas automatique. En effet, le gestionnaire de données ou le professionnel de l'informatique doit vérifier la connaissance dans la base de connaissances et les modifications à apporter aux données.  
  
 Après avoir créé le projet DQS initial, vous pouvez automatiser plusieurs tâches à l'aide des composants d' [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
 Pour plus d'informations, voir [Data Matching](../../data-quality-services/data-matching.md).  
  
 Tout en procédant aux activités de nettoyage et de mise en correspondance dans un projet de qualité de données, vous pouvez obtenir des statistiques et des informations en temps réel sur les données que DQS est entrain de traiter. Le profilage des données vous permet d'évaluer dans quelle mesure le nettoyage et la mise en correspondance des données vous ont aidé à améliorer la qualité des données, et aussi de comprendre les modifications qui ont été effectuées. Pour plus d'informations sur le profilage des données et les notifications, consultez [Data Profiling and Notifications in DQS](../../data-quality-services/data-profiling-and-notifications-in-dqs.md).  
  
 **Une base de connaissances qui représente trois types de connaissances : connaissance prête à l'emploi, connaissance générée par le serveur DQS et connaissance générée par l'utilisateur.**  
 Une fois que vous avez créé une base de connaissances, vous pouvez l'utiliser de manière itérative pour nettoyer et vérifier d'autres données.  
  
 Vous pouvez importer de nouvelles données dans les données de la base de connaissances, qui peuvent provenir de plusieurs sources. Il peut s'agir de données propres connues des fournisseurs de référence ou de données brutes qui sont mises en correspondance avec les données existantes de la base de connaissances.  
  
 Pour plus d'informations sur l'activité de nettoyage dans un projet de qualité des données, consultez Nettoyage de données (DQS).  
  
 Vous pouvez également appliquer la connaissance de la base de connaissances à d'autres sources pour procéder au nettoyage des données dans le cadre d'autres processus. Le nettoyage des données peut aider à identifier des erreurs de saisie des utilisateurs, des altérations dans la transmission ou le stockage, ou des incohérences dans les définitions du dictionnaire de données.  
  
 Pour plus d’informations, consultez [DQS Knowledge Bases and Domains](../../data-quality-services/dqs-knowledge-bases-and-domains.md).  
  
##  <a name="bkmk_FTSetc"></a> Recherche en texte intégral  
 La recherche en texte intégral dans SQL Server permet aux applications et aux utilisateurs d'exécuter des requêtes de texte intégral sur des données caractères dans des tables SQL Server. Lorsque la recherche en texte intégral est activée, vous pouvez effectuer des recherches sur les données texte qui sont améliorées grâce à des règles spécifiques à la langue sur les différentes formes d'un mot ou d'une expression. Vous pouvez également configurer des conditions de recherche, telles que la distance entre plusieurs termes, et utiliser des fonctions pour limiter les résultats retournés selon leur vraisemblance.  
  
 Étant donné que les requêtes de texte intégral sont une fonctionnalité fournie par le moteur SQL Server, vous pouvez créer des requêtes paramétrables et générer des jeux de données personnalisés ou des vecteurs de termes à l'aide des fonctionnalités de recherche en texte intégral sur une source de données de texte, puis utiliser ces sources dans l'exploration de données.  
  
 Pour plus d’informations sur les interactions entre les requêtes de texte intégral et l’index de recherche en texte intégral, consultez [Exécuter une requête avec une recherche en texte intégral](../../relational-databases/search/query-with-full-text-search.md).  
  
 L'avantage d'utiliser les fonctionnalités de texte intégral de SQL Server est que vous pouvez tirer parti de l'intelligence linguistique contenue dans les analyseurs lexicaux et les générateurs de formes dérivées qui sont proposés pour toutes les langues dans SQL Server. À l'aide des analyseurs lexicaux et des générateurs de formes dérivées, vous pouvez vous assurer que les mots sont séparés à l'aide des caractères appropriés de chaque langue, et que les synonymes selon les signes diacritiques ou les variations orthographiques (par exemple, les différents formats de nombre en japonais) ne sont pas négligés.  
  
 En plus de l'intelligence linguistique qui régit les limites des mots, les générateurs de formes dérivées de chaque langue peuvent réduire les variantes d'un mot à un terme unique, selon les règles de conjugaison et les variations orthographiques de chaque langue. Les règles pour l'analyse linguistique varient en fonction de chaque langue et sont développées sur la base de recherches étendues effectuées sur des références concrètes.  
  
 Pour plus d’informations, consultez [Configurer et gérer les analyseurs lexicaux et générateurs de formes dérivées pour la recherche](../../relational-databases/search/configure-and-manage-word-breakers-and-stemmers-for-search.md).  
  
 La version d'un mot stocké après l'indexation de texte intégral est un jeton au format compressé. Les requêtes suivantes de l'index de recherche en texte intégral génèrent plusieurs formes flexionnelles d'un mot particulier selon les règles d'une langue donnée pour s'assurer que toutes les correspondances potentielles sont effectuées. Par exemple, même si le jeton qui est stocké est « courir », le moteur de requête recherche également les termes « course », « couru » et « coureur », car ils correspondent à des variations morphologiques dérivées du mot racine « courir ».  
  
 Vous pouvez également créer et générer un dictionnaire des synonymes utilisateur pour stocker des synonymes et obtenir de meilleurs résultats de recherche ou un meilleur classement des termes. En développant un dictionnaire des synonymes adapté à vos données de texte intégral, vous pouvez élargir efficacement l'étendue des requêtes de texte intégral sur ces données. Pour plus d’informations, consultez [Configurer et gérer les fichiers de dictionnaire des synonymes pour la recherche en texte intégral](../../relational-databases/search/configure-and-manage-thesaurus-files-for-full-text-search.md).  
  
 Les spécifications liées à l'utilisation de la recherche en texte intégral sont les suivantes :  
  
-   L'administrateur de base de données doit créer un index de recherche en texte intégral sur la table.  
  
-   Un seul index de recherche en texte intégral est autorisé par table.  
  
-   Chaque colonne que vous indexez doit avoir une clé unique.  
  
-   L'indexation de recherche en texte intégral n'est pris en charge que pour les colonnes avec les types de données suivants : char, varchar, nchar, nvarchar, text, ntext, image, xml, varbinary et varbinary(max). Si la colonne est varbinary, varbinary (max), image ou xml, vous devez spécifier l'extension de fichier du document indexable (.doc, .pdf, .xls, etc.) dans une colonne de type distincte.  
  
##  <a name="bkmk_SemSearch"></a> Indexation sémantique  
 La recherche sémantique s'appuie sur les fonctionnalités de recherche en texte intégral existantes de SQL Server, mais utilise d'autres fonctions et statistiques pour permettre des scénarios tels que l'extraction automatique de mots clés et la détection de documents connexes. Par exemple, vous pouvez utiliser la recherche sémantique afin de générer une taxonomie simple pour une organisation ou classer un ensemble de documents. Vous pouvez également utiliser la combinaison des termes extraits et des scores de similarité des documents dans des modèles de clustering ou d'arbre de décision.  
  
 Après avoir activé la recherche sémantique et indexé vos colonnes de données, vous pouvez utiliser les fonctions fournies en mode natif avec l'indexation sémantique pour effectuer les opérations suivantes :  
  
-   Retourner des phrases clés unitermes avec leur score.  
  
-   Retourner des documents qui contiennent une phrase clé spécifiée.  
  
-   Retourner des scores de similarité et les termes qui contribuent au score.  
  
 Pour plus d’informations, consultez [Rechercher des expressions clés dans les documents avec la recherche sémantique](../../relational-databases/search/find-key-phrases-in-documents-with-semantic-search.md) et [Rechercher des documents similaires ou connexes avec la recherche sémantique](../../relational-databases/search/find-similar-and-related-documents-with-semantic-search.md).  
  
 Pour plus d’informations sur les objets de base de données qui prennent en charge l’indexation sémantique, consultez [Activer la recherche sémantique sur les tables et les colonnes](../../relational-databases/search/enable-semantic-search-on-tables-and-columns.md).  
  
 Les spécifications liées à l'utilisation de la recherche sémantique sont les suivantes :  
  
-   La recherche en texte intégral est également activée.  
  
-   L'installation des composants de recherche sémantique crée également une base de données système spéciale, qui ne peut pas être renommée, modifiée, ni remplacée.  
  
-   Les documents que vous indexez à l'aide du service doivent être stockés dans SQL Server, dans n'importe quel objet de base de données pris en charge pour l'indexation de texte intégral, notamment les tables et les vues indexées.  
  
-   Les langues de texte intégral ne prennent pas toutes en charge l'indexation sémantique. Pour obtenir la liste des langues prises en charge, consultez [sys.fulltext_semantic_languages &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-semantic-languages-transact-sql.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Solutions de modèles multidimensionnels ](../../analysis-services/multidimensional-models/multidimensional-model-solutions-ssas.md)   
 [Solutions de modèles tabulaires](../../analysis-services/tabular-models/tabular-models-ssas.md)  
  
  
