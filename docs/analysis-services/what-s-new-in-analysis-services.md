---
title: Ce que&#39;nouveauté dans Analysis Services | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: ''
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 859214876b5c62078ccdfee72bf23caf3904df07
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="what39s-new-in-analysis-services"></a>Ce que&#39;nouveauté dans Analysis Services
[!INCLUDE[ssas-appliesto-sql2016](../includes/ssas-appliesto-sql2016.md)]

SQL Server 2016 Analysis Services inclut de nombreuses nouvelles améliorations de fournir des performances améliorées, solution plus facile de création, de gestion automatisée de la base de données améliorée des relations avec bidirectionnelles entre le filtrage, le traitement des partitions parallèles et bien plus encore. Au cœur de la plupart des améliorations apportées à cette version se trouve le nouveau niveau de compatibilité 1200 pour les bases de données model tabulaires.     

## <a name="azure-analysis-services"></a>Azure Analysis Services
Comme nous l’avons annoncé lors de la conférence SQL PASS de 2016, Analysis Services est désormais disponible dans le cloud comme un service Azure. **Azure Analysis Services** prend en charge les modèles tabulaires aux niveaux de compatibilité 1200 et supérieurs. DirectQuery, les partitions, la sécurité de niveau ligne, les relations bidirectionnelles et les traductions sont toutes prises en charge. Pour en savoir plus et effectuer un essai gratuit, consultez [Azure Analysis Services](http://azure.microsoft.com/services/analysis-services/). 

## <a name="whats-new-in-sql-server-2016-service-pack-1-sp1-analysis-services"></a>Nouveautés dans SQL Server 2016 Service Pack 1 (SP1) Analysis Services

[Télécharger SQL Server 2016 SP1](http://www.microsoft.com/download/details.aspx?id=54276) 

SQL Server 2016 SP1 Analysis Services offre une amélioration des performances et de la scalabilité grâce à la reconnaissance NUMA (Non-Uniform Memory Access) et à l’allocation de mémoire optimisée en fonction des **blocs Intel TBB** (Intel Threading Building Blocks). Cette nouvelle fonctionnalité permet de réduire le coût total de possession en prenant en charge davantage d’utilisateurs sur une quantité moindre de serveurs d’entreprise plus puissants. 

En particulier, SQL Server 2016 SP1 Analysis Services offre des améliorations dans les domaines clés suivants :

-   **Reconnaissance NUMA** : pour une meilleure prise en charge de NUMA, le moteur (VertiPaq) en mémoire dans Analysis Services gère maintenant une file d’attente de travail distincte sur chaque nœud NUMA. Cela garantit que les travaux d’analyse de segment s’exécutent sur le même nœud que celui où la mémoire est allouée pour les données du segment. Notez que la reconnaissance NUMA est activée par défaut uniquement sur les systèmes comptant au moins quatre nœuds NUMA. Sur les systèmes à deux nœuds, les coûts d’accès à la mémoire allouée à distance ne justifient généralement pas la surcharge liée à la gestion des spécificités NUMA.
-   **Allocation de mémoire** : Analysis Services a été boosté avec Intel Threading Building Blocks, allocateur scalable qui fournit des pools de mémoire distincts pour chaque cœur. À mesure que le nombre de cœurs augmente, le système peut évoluer de manière quasi linéaire.
-   **Fragmentation des segments de mémoire** : l’allocateur scalable basé sur Intel TBB permet également d’atténuer les problèmes de performances dus à la fragmentation des segments de mémoire ayant été observés avec les segments de mémoire Windows.

Des tests de performances et de scalabilité ont montré des gains de débit de requête significatifs lors de l’exécution de SQL Server 2016 SP1 Analysis Services sur de grands serveurs d’entreprise à plusieurs nœuds.


## <a name="whats-new-in-sql-server-2016-analysis-services"></a>Nouveautés de SQL Server 2016 Analysis Services

Alors que la plupart des améliorations apportées à cette version sont spécifiques aux modèles tabulaires, un certain nombre ont été apportées aux modèles multidimensionnels, par exemple, l’optimisation ROLAP du comptage de valeurs pour les sources de données telles que DB2 et Oracle, la prise en charge de plusieurs sélections pour l’extraction avec Excel 2016 et les optimisations de requête Excel.    

#### <a name="get-the-latest-tools"></a>Récupérer les derniers outils
Pour tirer pleinement parti de toutes les améliorations dans cette version, veillez à installer les dernières versions de SSDT et SSMS.    
- [Télécharger SSDT (SQL Server Data Tools)](http://msdn.microsoft.com/library/mt204009.aspx)    
- [Télécharger SQL Server Management Studio (SSMS)](http://msdn.microsoft.com/library/mt238290.aspx)   

Si vous utilisez une application personnalisée dépendante d’AMO, vous devrez peut-être installer une version mise à jour d’AMO. Pour obtenir des instructions, consultez [Installer les fournisseurs de données Analysis Services &#40;AMO, ADOMD.NET, MSOLAP&#41;](../analysis-services/instances/install-windows/install-analysis-services-data-providers-amo-adomd-net-msolap.md).    

 #### <a name="technet-virtual-labs-sql-server-2016-analysis-services"></a>Ateliers pratiques virtuels TechNet : SQL Server 2016 Analysis Services
Rien de tel que la pratique ? Suivez les procédures proposées dans [l’atelier pratique consacré aux nouveautés de SQL Server 2016 Analysis Services](http://vlabs.holsystems.com/vlabs/technet?eng=VLabs&auth=none&src=vlabs&altadd=true&labid=23110&lod=true).
Dans cet atelier, vous allez créer et surveiller des événements étendus (xEvents), mettre à niveau un projet tabulaire vers le niveau de compatibilité 1200, utiliser des configurations de Visual Studio, implémenter de nouvelles fonctionnalités de calcul, implémenter de nouvelles fonctionnalités de relation entre les tables, configurer des dossiers d’affichage, gérer les traductions de modèle, utiliser le nouveau langage TMSL (Tabular Model Scripting Language), utiliser PowerShell et essayer de nouvelles fonctionnalités du mode DirectQuery.

## <a name="modeling"></a>Modélisation    
### <a name="improved-modeling-performance-for-tabular-1200-models"></a>Amélioration des performances de modélisation pour les modèles 1200 tabulaires    
Pour les modèles 1200 tabulaires, les opérations de métadonnées effectuées dans SSDT sont beaucoup plus rapides que les modèles tabulaires 1100 ou 1103. À titre de comparaison, sur le même matériel, la création d’une relation sur un modèle défini au niveau de compatibilité de SQL Server 2014 (1103) avec 23 tables prend trois secondes, alors que la même relation sur un modèle défini au niveau de compatibilité 1200 prend à peine une seconde.    
### <a name="project-templates-added-for-tabular-1200-models-in-ssdt"></a>Ajout de modèles de projet pour les modèles 1200 tabulaires dans SSDT    
Avec cette version, vous n’avez plus besoin de deux versions de SSDT pour créer des projets BI et des projets relationnels. [SQL Server Data Tools pour Visual Studio 2015](http://msdn.microsoft.com/library/mt204009.aspx) ajoute des modèles de projet pour les solutions Analysis Services, y compris les **projets tabulaires Analysis Services** utilisés pour créer des modèles au niveau de compatibilité 1200. D’autres modèles de projet Analysis Services pour les solutions multidimensionnelles et d’exploration de données sont également fournis, mais au même niveau de compatibilité (1100 ou 1103) que dans les versions précédentes.    
### <a name="display-folders"></a>Dossiers d’affichage
Les dossiers d’affichage sont désormais disponibles pour les modèles 1200 tabulaires. Définis dans SQL Server Data Tools et affichés dans les applications clientes comme Excel ou Power BI Desktop, les dossiers d’affichage vous permettent d’organiser facilement des mesures en grand nombre dans des dossiers individuels. De cette façon, les mesures sont présentées de façon hiérarchique, ce qui simplifie la navigation dans les listes de champs.
### <a name="bi-directional-cross-filtering"></a>Filtrage croisé bidirectionnel
L’une des nouveautés de cette version est son approche intégrée d’activation des filtres croisés bidirectionnels dans les modèles tabulaires. Grâce à elle, plus besoin de concevoir manuellement des solutions de contournement DAX pour propager un contexte de filtre dans les relations de table. Les filtres sont créés automatiquement uniquement si la direction des filtres peut être établie avec un haut degré de certitude. S’il existe une ambiguïté en ce qui concerne le format de plusieurs chemins de requête sur les relations de table, les filtres ne sont pas créés automatiquement. Pour plus d’informations, consultez [Filtres croisés bidirectionnels pour modèles tabulaires dans SQL Server 2016 Analysis Services](../analysis-services/tabular-models/bi-directional-cross-filters-tabular-models-analysis-services.md) .
 ### <a name="translations"></a>Traductions    
 Vous pouvez désormais stocker des métadonnées traduites dans un modèle 1200 tabulaire. Les métadonnées du modèle incluent des champs pour la **Culture**, les légendes traduites et les descriptions traduites. Pour ajouter des traductions, utilisez la commande **Modèle** > **Traductions** dans [!INCLUDE[ssBIDevStudio](../includes/ssbidevstudio-md.md)]. Pour plus d’informations, consultez [Traductions dans les modèles tabulaires &#40;Analysis Services&#41;](../analysis-services/tabular-models/translations-in-tabular-models-analysis-services.md).    
 ### <a name="pasted-tables"></a>Tables collées    
 Si vous utilisez un modèle tabulaire 1100 ou 1103 qui contient des tables collées, vous pouvez désormais le mettre à niveau vers un modèle 1200. Nous vous recommandons d’utiliser [!INCLUDE[ssBIDevStudio](../includes/ssbidevstudio-md.md)]. Dans SSDT, définissez **CompatibilityLevel** à la valeur 1200 et effectuez le déploiement sur une instance [!INCLUDE[ssCurrent](../includes/sscurrent-md.md)] d’ [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)]. Pour plus d’informations, consultez [Compatibility Level for Tabular models in Analysis Services](../analysis-services/tabular-models/compatibility-level-for-tabular-models-in-analysis-services.md) .    
 ### <a name="calculated-tables-in-ssdt"></a>Tables calculées dans SSDT    
Une *table calculée* est une construction de modèle uniquement qui est basée sur une expression ou requête DAX dans SSDT. Quand une table calculée est déployée dans une base de données, elle ne se distingue pas des tables standard.    

 Les tables calculées ont plusieurs utilisations, notamment la création de tables pour exposer une table existante dans un rôle spécifique. L’exemple type est une table de dates qui s’utilise dans plusieurs contextes (date de commande, date d’expédition, etc.). En créant une table calculée pour un rôle donné, vous pouvez désormais établir une relation de table pour faciliter les requêtes ou les interactions de données avec cette table calculée. Une autre utilisation possible des tables calculées est de combiner certains éléments de tables existantes dans une toute nouvelle table qui existe uniquement dans le modèle.  Consultez [créer une Table calculée](../analysis-services/tabular-models/create-a-calculated-table-ssas-tabular.md) pour en savoir plus.    
 ### <a name="formula-fixup"></a>Correction de formule    
 Avec la correction de formule sur un modèle tabulaire 1200, SSDT met automatiquement à jour les mesures qui font référence à une colonne ou table ayant été renommée.    
 ### <a name="support-for-visual-studio-configuration-manager"></a>Prise en charge du Gestionnaire de configuration Visual Studio    
 Pour garantir la prise en charge de plusieurs environnements, comme des environnements de test et de pré-production, Visual Studio permet aux développeurs de créer plusieurs configurations de projet à l’aide du Gestionnaire de configuration. Les modèles multidimensionnels offrent déjà cette possibilité, mais ce n’était pas le cas des modèles tabulaires. Avec cette version, vous pouvez utiliser le gestionnaire de configuration pour effectuer un déploiement sur différents serveurs.    

## <a name="instance-management"></a>Gestion d’instances    
 ### <a name="administer-tabular-1200-models-in-ssms"></a>Administration de modèles 1200 tabulaires dans SSMS    
 Dans cette version, une instance d’Analysis Services en mode serveur tabulaire peut exécuter les modèles tabulaires à tous les niveaux de compatibilité (1100, 1103, 1200). La dernière version de [SQL Server Management Studio](http://msdn.microsoft.com/library/mt238290.aspx) a été mise à jour pour afficher les propriétés et permettre l’administration de modèles de base de données pour les modèles tabulaires au niveau de compatibilité 1200.    
 ### <a name="parallel-processing-for-multiple-table-partitions-in-tabular-models"></a>Traitement en parallèle de plusieurs partitions de tables dans les modèles tabulaires    
 Cette version inclut une nouvelle fonctionnalité de traitement en parallèle des tables avec plusieurs partitions, ce qui améliore les performances de traitement. Il n’y a pas de paramètres de configuration pour cette fonctionnalité. Pour plus d’informations sur la configuration des partitions et de traiter les tables, consultez [des Partitions de modèles tabulaires](../analysis-services/tabular-models/tabular-model-partitions-ssas-tabular.md).    
 ### <a name="add-computer-accounts-as-administrators-in-ssms"></a>Ajout de comptes d’ordinateur en tant qu’administrateurs dans SSMS    
 Les administrateurs de[!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] peuvent désormais utiliser [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] pour configurer des comptes d’ordinateur en tant que membres du groupe d’administrateurs [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] . Dans la boîte de dialogue **Sélectionner des utilisateurs ou des groupes** , définissez l’option **Emplacements** pour le domaine des ordinateurs, puis ajoutez le type d’objet **Ordinateurs** . Pour plus d’informations, consultez [Accorder des droits d’administrateur de serveur à une instance Analysis Services](../analysis-services/instances/grant-server-admin-rights-to-an-analysis-services-instance.md).    
 ### <a name="dbcc-for-analysis-services"></a>DBCC pour Analysis Services    
 DBCC (Database Consistency Checker) s’exécute en interne pour détecter la présence potentielle de données endommagées au chargement d’une base de données, mais il peut aussi être exécuté à la demande si vous soupçonnez des problèmes dans vos données ou un modèle. DBCC exécute des vérifications différentes selon que le modèle est tabulaire ou multidimensionnel. Pour plus de détails, consultez [DBCC &#40;Database Consistency Checker&#41; pour les bases de données multidimensionnelles et tabulaires Analysis Services](../analysis-services/instances/database-consistency-checker-dbcc-for-analysis-services.md).    
 ### <a name="extended-events-updates"></a>Mises à jour des événements étendus    
 Cette version ajoute une interface utilisateur graphique à [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] pour la configuration et la gestion des événements étendus [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] . Vous pouvez configurer des flux de données actifs pour surveiller l’activité du serveur en temps réel, conserver les données de session chargées en mémoire pour une analyse plus rapide, ou enregistrer les flux de données dans un fichier pour une analyse hors connexion. Pour plus d’informations, consultez [Surveiller Analysis Services avec des événements étendus SQL Server](../analysis-services/instances/monitor-analysis-services-with-sql-server-extended-events.md) , ainsi que [la vidéo et le billet de blog GuyInACube sur l’utilisation des événements étendus avec Analysis Services](http://blogs.msdn.com/b/analysisservices/archive/2015/09/22/using-extended-events-with-sql-server-analysis-services-2016-cpt-2-3.aspx).    



## <a name="scripting"></a>Création de scripts
 ### <a name="powershell-for-tabular-models"></a>PowerShell pour les modèles tabulaires    
 Cette version inclut des améliorations de PowerShell pour les modèles tabulaires au niveau de compatibilité 1200. Vous pouvez utiliser toutes les applets de commande applicables, ainsi que les applets de commande spécifiques du mode tabulaire : [Invoke-ProcessASDatabase](../analysis-services/powershell/invoke-processasdatabase.md) et [Invoke-ProcessTable](../analysis-services/powershell/invoke-processtable-cmdlet.md).    
 ### <a name="ssms-scripting-database-operations"></a>Création de scripts SSMS pour les opérations de base de données    
 Dans la [dernière version de SQL Server Management Studio (SSMS)](http://msdn.microsoft.com/library/mt238290.aspx), la création de scripts est désormais possible pour les commandes de base de données, notamment Create, Alter, Delete, Backup, Restore, Attach et Detach. La sortie est en langage TMSL (Tabular Model Scripting Language) au format JSON. Pour plus d’informations, consultez [Tabular Model Scripting Language &#40;TMSL&#41; Reference](../analysis-services/tabular-model-scripting-language-tmsl-reference.md) (Informations de référence sur TMSL&#40;Tabular Model Scripting Language&#41;).    
 ### <a name="analysis-services-execute-ddl-task"></a>Tâche DDL d'exécution de SQL Server Analysis Services    
 La[tâche DDL d’exécution Analysis Services](../integration-services/control-flow/analysis-services-execute-ddl-task.md) prend aussi désormais en charge les commandes en langage TMSL (Tabular Model Scripting Language).     
 ### <a name="ssas-powershell-cmdlet"></a>Applet de commande PowerShell de SSAS    
 L’applet de commande PowerShell **Invoke-ASCmd** de SSAS prend maintenant en charge les commandes en langage TMSL (Tabular Model Scripting Language). D’autres applets de commande PowerShell de SSAS pourraient être prises en charge dans une prochaine version pour pouvoir utiliser les nouvelles métadonnées tabulaires (les exceptions seront signalées dans les notes de publication).    
Pour plus d'informations, consultez [Analysis Services PowerShell Reference](../analysis-services/powershell/analysis-services-powershell-reference.md) .    
 ### <a name="tabular-model-scripting-language-tmsl-supported-in-ssms"></a>Prise en charge du langage TMSL (Tabular Model Scripting Language) dans SSMS    
  Avec la [version la plus récente de SSMS](http://msdn.microsoft.com/library/mt238290.aspx), vous pouvez maintenant créer des scripts pour automatiser la plupart des tâches d’administration pour les modèles 1200 tabulaires. Actuellement, vous pouvez créer des scripts pour les tâches suivantes : Process à tous les niveaux, ainsi que CREATE, ALTER et DELETE au niveau de la base de données.    
    
 TMSL fonctionne de manière équivalente à l’extension ASSL de XMLA qui fournit les définitions des objets multidimensionnels. La différence est que TMSL utilise des descripteurs natifs, tels que **model**, **table**et **relationship** , pour décrire les métadonnées tabulaires. Pour plus d’informations sur le schéma, consultez [Tabular Model Scripting Language &#40;TMSL&#41; Reference](../analysis-services/tabular-model-scripting-language-tmsl-reference.md) (Informations de référence sur TMSL&#40;Tabular Model Scripting Language&#41;).    
    
 Un script au format JSON créé pour un modèle tabulaire ressemble à ceci :    
    
```    
{    
  "create": {    
    "database": { 
      "name": "AdventureWorksTabular1200",    
      "id": "AdventureWorksTabular1200",    
      "compatibilityLevel": 1200,    
      "readWriteMode": "readWrite",    
      "model": {}    
    }    
  }    
}    
```    

La charge utile est un document JSON qui peut être très simple, comme dans l’exemple ci-dessus, ou être beaucoup plus complet avec l’ensemble des définitions d’objet. Pour plus d’informations sur la syntaxe, consultez [Tabular Model Scripting Language &#40;TMSL&#41; Reference](../analysis-services/tabular-model-scripting-language-tmsl-reference.md) (Informations de référence sur TMSL&#40;Tabular Model Scripting Language&#41;).

Au niveau de la base de données, le script TMSL pour les commandes CREATE, ALTER et DELETE est généré dans la fenêtre XMLA classique.  Dans cette version, d’autres commandes, telles que Process, peuvent également faire l’objet d’un script. La prise en charge des scripts pour de nombreuses autres actions pourrait être ajoutée dans une version ultérieure.    

**Commandes autorisées dans un script** | **Description**
--------------- | ----------------
create|Ajoute une base de données, connexion ou partition. L’équivalent ASSL est CREATE.
createOrReplace|Met à jour une définition d’objet existante (base de données, connexion ou partition) en remplaçant une version précédente. L’équivalent ASSL est ALTER avec AllowOverwrite défini sur true et ObjectDefinition défini sur ExpandFull.
delete|Supprime une définition d’objet. L’équivalent ASSL est DELETE.
refresh|Traite l'objet. L’équivalent ASSL est PROCESS.

## <a name="dax"></a>DAX
### <a name="improved-dax-formula-editing"></a>Amélioration de la modification des formules DAX
La barre de formule mise à jour facilite l’écriture de formules grâce aux améliorations suivantes : des couleurs de syntaxe pour différencier les fonctions, les champs et les mesures, un mode intelligent de suggestions des fonctions et champs, et l’affichage de *tildes*d’erreur pour indiquer une syntaxe incorrecte dans votre expression DAX. Vous pouvez également insérer plusieurs lignes (Alt+Entrée) et des retraits (Tab) dans vos formules. Dans la barre de formule, vous pouvez aussi écrire des commentaires directement dans vos mesures. Tapez simplement deux barres obliques (« // ») pour mettre en commentaire tout le texte suivant ces caractères sur la même ligne.

### <a name="dax-variables"></a>Variables DAX    
Cette version inclut désormais la prise en charge des variables DAX. Les variables peuvent maintenant stocker le résultat d’une expression comme une variable nommée, qui peut ensuite être passée en tant qu’argument à d’autres expressions de mesure. Une fois que les valeurs résultantes ont été calculées pour une expression de variable, ces valeurs ne changent pas, même si la variable est référencée dans une autre expression. Pour plus d’informations, consultez [Fonction VAR](http://msdn.microsoft.com/library/mt243785.aspx).    
### <a name="new-dax-functions"></a>Nouvelles fonctions DAX
Avec cette version, DAX introduit plus de cinquante nouvelles fonctions, qui accélèrent les calculs et améliorent les visualisations dans Power BI. Pour plus d’informations, consultez [New DAX Functions](http://msdn.microsoft.com/library/mt704075.aspx)(Nouvelles fonctions DAX).
### <a name="save-incomplete-measures"></a>Enregistrement des mesures incomplètes
Vous pouvez maintenant enregistrer les mesures DAX incomplètes directement dans un projet de modèle 1200 tabulaire et les terminer plus tard.
### <a name="additional-dax-enhancements"></a>Autres améliorations DAX
- Calcul non vide : réduit le nombre d’analyses nécessaires pour les calculs non vides.
- Fusion de mesures : plusieurs mesures de la même table sont combinées dans une même requête du moteur de stockage.
- Regroupement des jeux : quand une requête demande des mesures impliquant plusieurs granularités (total/année/mois), une seule requête est envoyée au niveau le plus bas et le reste des granularités sont dérivées de ce niveau.     
- Élimination des jointures redondantes : une seule requête vers le moteur de stockage retourne les colonnes de dimension et les valeurs de mesure.
- Évaluation stricte de IF/SWITCH : une branche dont la condition a la valeur false ne provoque plus de requêtes du moteur de stockage. Auparavant, les branches étaient évaluées de manière hâtive, mais les résultats étaient ignorés plus tard.     
    
## <a name="developer"></a>Développeur    
 ### <a name="microsoftanalysisservicestabular-namespace-for-tabular-1200-programmability-in-amo"></a>Espace de noms Microsoft.AnalysisServices.Tabular pour la programmabilité d’objets tabulaires 1200 dans AMO
 Les objets Analysis Services Management (AMO) ont été mis à jour. Ils incluent un nouvel espace de noms tabulaire pour gérer une instance en mode tabulaire de SQL Server 2016 Analysis Services, et fournissent le langage de définition de données (DLL) nécessaire pour créer ou modifier des modèles 1200 tabulaires par programmation. Pour plus d’informations sur l’API, consultez [Microsoft.AnalysisServices.Tabular](http://msdn.microsoft.com/library/microsoft.analysisservices.tabular.aspx) .    
 ### <a name="analysis-services-management-objects-amo-updates"></a>Mises à jour d’Analysis Services Management Objects (AMO)
 [Analysis Services Management Objects &#40;AMO&#41;](http://msdn.microsoft.com/library/mt436122.aspx) a été refactorisé pour inclure un deuxième assembly, Microsoft.AnalysisServices.Core.dll. Le nouvel assembly sépare les classes communes (telles que Server, Database et Role) qui ont un champ d’application étendu dans Analysis Services, quel que soit le mode du serveur utilisé.    
    
 Auparavant, ces classes faisaient partie de l'assembly Microsoft.AnalysisServices d’origine. En les déplaçant vers un nouvel assembly, cela rend possible de futures extensions vers AMO, grâce à une séparation claire entre les API génériques et les API contextuelles.    
    
 Les applications existantes ne sont pas affectées par les nouveaux assemblys. Toutefois, si vous décidez de régénérer les applications avec le nouvel assembly AMO, assurez-vous d’ajouter une référence à Microsoft.AnalysisServices.Core.    
    
 De la même façon, les scripts PowerShell qui chargent et appellent AMO doivent charger Microsoft.AnalysisServices.Core.dll. Veillez à mettre à jour tous les scripts.  

### <a name="json-editor-for-bim-files"></a>Éditeur JSON pour les fichiers BIM
Le mode Code dans Visual Studio 2015 affiche maintenant le fichier BIM au format JSON pour les modèles 1200 tabulaires. La version de Visual Studio détermine si les fichiers BIM sont rendus au format JSON via l’éditeur JSON intégré ou sous forme de texte simple.

Pour utiliser l’éditeur JSON avec la possibilité de développer et réduire des sections du modèle, vous avez besoin de la dernière version de SQL Server Data Tools et de Visual Studio 2015 (n’importe quelle édition, y compris l’édition Community gratuite). Dans toutes les autres versions de SSDT ou de Visual Studio, les fichiers BIM sont rendus au format JSON sous forme de texte simple.
Au minimum, un modèle vide contient le texte JSON suivant :

    ```    
    {    
      "name": "SemanticModel",
      "id": "SemanticModel",
      "compatibilityLevel": 1200,
      "readWriteMode": "readWrite",
      "model": {}
    }    
    ```    
    
> [!WARNING]    
> Évitez de modifier le fichier JSON directement, car cela peut endommager le modèle.    
 ### <a name="new-elements-in-ms-csdlbi-20-schema"></a>Nouveaux éléments dans le schéma MS-CSDLBI 2.0    
 Les éléments suivants ont été ajoutés au type complexe **TProperty** défini dans le schéma [MS-CSDLBI] 2.0 :    
    
|Élément|Définition|    
|-------------|----------------|    
|DefaultValue|Propriété qui spécifie la valeur utilisée pour évaluer la requête. La propriété DefaultValue est facultative, mais elle est automatiquement sélectionnée si les valeurs du membre ne peuvent pas être agrégées.|    
|Statistiques|Ensemble de statistiques effectuées sur les données sous-jacentes qui sont associées à la colonne. Ces statistiques, définies par le type complexe TPropertyStatistics, sont fournies uniquement si cela ne nécessite pas une grande quantité de ressources de calcul, comme cela est expliqué dans la section 2.1.13.5 sur le format CSDL (Conceptual Schema Definition Language) dans le document Business Intelligence Annotations.|    
    
## <a name="directquery"></a>DirectQuery    
### <a name="new-directquery-implementation"></a>Nouvelle implémentation de DirectQuery    
Cette version comporte des améliorations significatives dans DirectQuery pour les modèles 1200 tabulaires. En voici un résumé :    
-   DirectQuery crée désormais des requêtes plus simples qui offrent de meilleures performances.    
-   Contrôle accru de la définition d’exemples de datasets utilisés pour la conception et le test des modèles.    
-   La sécurité au niveau des lignes (RLS) est maintenant prise en charge pour les modèles 1200 tabulaires en mode DirectQuery. Avant, l’activation de la sécurité au niveau des lignes empêchait le déploiement d’un modèle tabulaire en mode DirectQuery.    
-   Les colonnes calculées sont maintenant prises en charge pour les modèles 1200 tabulaires en mode DirectQuery. Avant, l’activation des colonnes calculées empêchait le déploiement d’un modèle tabulaire en mode DirectQuery.    
-   L’optimisation des performances inclut l’élimination des jointures redondantes pour VertiPaq et DirectQuery. 

### <a name="new-data-sources-for-directquery-mode"></a>Nouvelles sources de données pour le mode DirectQuery    
 Sources de données pris en charge pour les modèles tabulaires 1200 dans le mode DirectQuery maintenant incluent Oracle, Teradata et Microsoft Analytique Platform (anciennement Parallel Data Warehouse).    
    
Pour plus d’informations, consultez [DirectQuery Mode](../analysis-services/tabular-models/directquery-mode-ssas-tabular.md).    

## <a name="see-also"></a>Voir aussi
[Blog de l’équipe Analysis Services](http://blogs.msdn.microsoft.com/analysisservices/)    
[Nouveautés de SQL Server 2016](../sql-server/what-s-new-in-sql-server-2016.md)    
     

