---
title: Comparaison des Solutions multidimensionnelles et tabulaires (SSAS) | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: ''
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 4e6a5941b33d2d73ee8bd86e33a710e065a21ae1
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="comparing-tabular-and-multidimensional-solutions"></a>Comparaison des solutions multidimensionnelles et tabulaires
[!INCLUDE[ssas-appliesto-sqlas](../includes/ssas-appliesto-sqlas.md)]

  SQL Server Analysis Services offre plusieurs approches pour la création d’un modèle sémantique business intelligence : tabulaire, multidimensionnel et PowerPivot pour SharePoint.
  
 Le fait d’avoir plusieurs approches offre une expérience de modélisation adaptée aux différents besoins des entreprises et des utilisateurs. Le modèle multidimensionnel est une technologie rodée reposant sur des normes ouvertes, adoptée par de nombreux éditeurs de logiciels d’aide à la décision (Business Intelligence ou BI), mais il peut être difficile à maîtriser. Le modèle tabulaire propose une approche de modélisation relationnelle que de nombreux développeurs jugent plus intuitive. Le modèle PowerPivot est encore plus simple, car il offre une modélisation visuelle des données dans Excel, avec prise en charge des serveurs fournie via SharePoint.  
  
 Tous les modèles sont déployés en tant que bases de données qui s’exécutent sur une instance d’Analysis Services, accessible aux outils clients à l’aide d’un ensemble de fournisseurs de données, et visualisable dans des rapports interactifs et statiques via Excel, Reporting Services, Power BI et des outils BI d’autres fournisseurs.  
  
 Les solutions tabulaires et multidimensionnelles sont créées à l’aide de SSDT et sont destinées aux projets de BI d’entreprise qui s’exécutent sur un serveur autonome de [!INCLUDE[ssASnoversion](../includes/ssasnoversion-md.md)] de l’instance locale et pour les modèles tabulaires, une [Azure Analysis Services](https://azure.microsoft.com/services/analysis-services/) serveur dans le cloud. Les deux solutions fournissent des bases de données analytiques hautes performances qui s’intègrent aisément aux clients BI. Pourtant, chaque solution diffère dans la façon dont elle est créée, utilisée et déployée. L’essentiel de cette rubrique compare ces deux types afin que vous puissiez identifier l’approche qui vous convient.  
  
 Pour les nouveaux projets, nous déconseillons généralement de modèles tabulaires. Les modèles tabulaires sont plus rapides à concevoir, tester et déployer ; et seront fonctionne mieux avec les dernières applications libre-service BI et les services de Microsoft cloud.  
  
##  <a name="bkmk_overview"></a> Vue d’ensemble des types de modélisation  
 Vous découvrez Analysis Services ? Le tableau suivant énumère les différents modèles, résume l’approche et identifie le support de diffusion initial.  
 
 > [!NOTE]  
>  **Azure Analysis Services** prend en charge les modèles tabulaires aux niveaux de compatibilité 1200 et supérieurs. Toutefois, toutes les fonctionnalités de modélisation tabulaire décrites dans cette rubrique sont pris en charge dans Azure Analysis Services. Lors de la création et le déploiement de modèles tabulaires à Azure Analysis Services sont la même que pour local, il est important de comprendre les différences. Pour plus d’informations, consultez [Nouveautés Azure Analysis Services ?](https://docs.microsoft.com/azure/analysis-services/analysis-services-overview)
  
||||  
|-|-|-|  
|**Type**|**Description de la modélisation**|**Diffusion**|  
|Tabulaire|Constructions de modélisation relationnelle (modèle, tables, colonnes). En interne, les métadonnées sont héritées de constructions de modélisation OLAP (cubes, dimensions, mesures). Le code et les scripts utilisent des métadonnées OLAP.|SQL Server 2012 et versions ultérieures (niveaux de compatibilité 1050-1103) <sup>1</sup>|  
|Tabulaire dans SQL Server 2016|Relationnelle constructions (modèle, tables, colonnes), exprimées dans les définitions des objets dans les métadonnées tabulaires de modélisation [TMSL Tabular Model Scripting Language ()](../analysis-services/tabular-model-scripting-language-tmsl-reference.md) et [le modèle d’objet tabulaire (TOM)](../analysis-services/tabular-model-programming-compatibility-level-1200/introduction-to-the-tabular-object-model-tom-in-analysis-services-amo.md) code.|SQL Server 2016 (niveau de compatibilité 1200)| 
|Tabulaire dans SQL Server 2017|Relationnelle constructions (modèle, tables, colonnes), exprimées dans les définitions des objets dans les métadonnées tabulaires de modélisation [TMSL Tabular Model Scripting Language ()](../analysis-services/tabular-model-scripting-language-tmsl-reference.md) et [le modèle d’objet tabulaire (TOM)](../analysis-services/tabular-model-programming-compatibility-level-1200/introduction-to-the-tabular-object-model-tom-in-analysis-services-amo.md) code.|SQL Server 2017 (niveau de compatibilité 1400)| 
|(Multidimensionnel)|Constructions de modélisation OLAP (cubes, dimensions, mesures).|SQL Server 2000 et versions ultérieures|  
|Power Pivot|À l’origine un complément, désormais entièrement intégré dans Excel. Modélisation uniquement, sur une infrastructure tabulaire interne. Vous pouvez importer un modèle PowerPivot dans SSDT pour créer un modèle tabulaire s’exécutant sur une instance Analysis Services.|Via Excel et Power BI Desktop|  
  
 <sup>1</sup> niveaux de compatibilité sont importants dans la version actuelle, en raison du moteur de métadonnées tabulaires et prise en charge de l’activation de scénario de fonctionnalités disponibles uniquement au niveau supérieur. Les versions ultérieures prennent en charge les premiers niveaux de compatibilité, mais il est recommandé de créer de nouveaux modèles ou de la mise à niveau des modèles existants vers le plus haut niveau de compatibilité pris en charge par la version du serveur.
  
##  <a name="bkmk_models"></a> Fonctionnalités de modèle  
  Le tableau suivant résume la disponibilité des fonctionnalités au niveau du modèle. Passez en revue cette liste pour vérifier que la fonctionnalité que vous voulez utiliser est disponible dans le type de modèle que vous prévoyez de créer.  
  
|||| 
|-|-|-|
||(Multidimensionnel)|Tabulaire|
|Actions|Oui|non|
|Aggregations|Oui|non|
|Colonne calculée|non|Oui|  
|Mesures calculées|Oui|Oui| 
|Tables calculées|non|Oui<sup>1</sup>|  
|Assemblys personnalisés|Oui|non|
|Cumuls personnalisés|Oui|non| 
|Membre par défaut|Oui|non|  
|Dossiers d’affichage|Oui|Oui<sup>1</sup>|  
|Distinct Count|Oui|Oui (via DAX)|
|extraction|Oui|Oui (dépend de l’application cliente)|
|Hierarchies|Oui|Oui|
|Indicateurs de performance clés|Oui|Oui| 
|Objets liés|Oui|Oui (tables liées)|
|Expressions de M|non|Oui<sup>1</sup>|
|Relations plusieurs-à-plusieurs|Oui|Non (mais il est [bidirectionnelles entre les filtres](../analysis-services/tabular-models/bi-directional-cross-filters-tabular-models-analysis-services.md) niveaux de compatibilité 1200 et versions ultérieures)| 
|Les jeux nommés|Oui|non| 
|Hiérarchies déséquilibrées|Oui|Oui<sup>1</sup>|  
|Hiérarchies parent-enfant|Oui|Oui (via DAX)|
|Partitions|Oui|Oui| 
|Perspectives|Oui|Oui|
|Sécurité de niveau ligne|Oui|Oui| 
|Sécurité au niveau de l’objet|Oui|Oui<sup>1</sup>|
|Mesures semi-additives|Oui|Oui| 
|Traductions|[Oui](../analysis-services/multidimensional-models/translations-in-multidimensional-models-analysis-services.md)|Oui| 
|Hiérarchies définies par l'utilisateur|Oui|Oui|
|Écriture différée|Oui|non| 
  
 <sup>1</sup> consultez [Compatibility Level for Tabular models dans Analysis Services](../analysis-services/tabular-models/compatibility-level-for-tabular-models-in-analysis-services.md) pour plus d’informations sur les différences fonctionnelles entre les niveaux de compatibilité.  
  
##  <a name="bkmk_ds"></a> Considérations relatives aux données  
 Les modèles tabulaires et multidimensionnelles utilisent des données importées à partir de sources externes. La quantité et le type de données que vous devez importer peuvent être des aspects essentiels à prendre en considération lors du choix du modèle le plus adapté à vos données.  
  
 **Compression**  
  
 Les solutions tabulaires et multidimensionnelles utilisent la compression des données, ce qui réduit la taille de la base de données Analysis Services par rapport à l'entrepôt de données à partir duquel vous importez les données. Étant donné que la compression réelle varie en fonction des caractéristiques des données sous-jacentes, il n'existe aucun moyen de connaître précisément la quantité d'espace disque et de mémoire requise par une solution après que les données ont été traitées et utilisées dans des requêtes.  
  
 Une estimation utilisée par de nombreux développeurs Analysis Services est que le stockage principal d'une base de données multidimensionnelle sera environ équivalent à un tiers de la taille des données d'origine. Les bases de données tabulaires peuvent parfois obtenir un taux de compression plus élevé, environ un dixième de la taille et ce, plus particulièrement si la plupart des données est importée depuis des tables de faits.  
  
 **Taille du modèle et écart de ressource (en mémoire ou sur disque)**  
  
 La taille d’une base de données Analysis Services est limitée uniquement par les ressources disponibles pour l’exécuter. Le mode de stockage et le type de modèle déterminent également la taille que peut atteindre la base de données.  
  
 Les bases de données tabulaires s’exécutent soit en mémoire, soit en mode DirectQuery qui permet de décharger l’exécution des requêtes vers une base de données externe. Pour l’analytique tabulaire en mémoire, la base de données est stocké entièrement en mémoire, ce qui signifie que vous devez avoir suffisamment de mémoire non seulement charger toutes les données, mais également des structures de données supplémentaires créées pour prendre en charge les requêtes.  
  
 DirectQuery, revisité dans SQL Server 2016, présente moins de restrictions qu’auparavant et améliorer les performances. Le fait de tirer parti de la base de données relationnelle principale pour le stockage et l’exécution des requêtes rend la génération d’un modèle tabulaire à grande échelle plus réalisable qu’auparavant.  
  
 Historiquement, les bases de données volumineuses en production sont multidimensionnelles, dont les charges de traitement et de requête en cours d’exécution indépendamment sur du matériel dédié, chacune étant optimisée pour son propre usage.  Les bases de données tabulaires rattrapent rapidement leur retard, et de nouvelles avancées dans DirectQuery contribuent à combler l’écart encore davantage.  
  
 Pour le stockage de données multidimensionnelles déchargement de requêtes et de l’exécution est disponible via ROLAP.   Un serveur de requêtes permet de mettre en cache des ensembles de lignes et de paginer mes ensembles caduques. L’utilisation efficace et équilibrée des ressources de mémoire et de disque guide souvent les clients vers des solutions multidimensionnelles.  
  
 Lors du chargement, la configuration requise pour le disque et la mémoire pour les deux types de solutions peut être augmentée à mesure qu'Analysis Services met en cache, stocke, analyse et interroge les données. Pour plus d’informations sur les options de pagination de mémoire, voir [Memory Properties](../analysis-services/server-properties/memory-properties.md). Pour en savoir plus sur l’échelle, voir [High availability and Scalability in Analysis Services](../analysis-services/instances/high-availability-and-scalability-in-analysis-services.md).  
  
 PowerPivot pour Excel a une limite de taille de fichier artificielle de 2 gigaoctets, qui est imposée de façon à ce que les classeurs créés dans PowerPivot pour Excel puissent être téléchargés sur SharePoint, qui définit des limites maximales sur la taille de téléchargement des fichiers. Une des principales raisons de la migration d’un classeur PowerPivot vers une solution tabulaire sur une instance Analysis Services autonome réside est le contournement de cette limitation de taille de fichier. Pour plus d’informations sur la configuration de la taille maximale de téléchargement de fichiers, consultez [Configurer la taille maximale de téléchargement de fichiers &#40;PowerPivot pour SharePoint&#41;](../analysis-services/power-pivot-sharepoint/configure-maximum-file-upload-size-power-pivot-for-sharepoint.md).  
  
 **Sources de données prises en charge**  
  
 Les modèles tabulaires peuvent importer des données à partir de sources de données relationnelles, de flux de données et de certains formats de documents. Vous pouvez également utiliser OLE DB pour des fournisseurs ODBC avec les modèles tabulaires. Les modèles tabulaires au niveau de compatibilité 1400 offrent une augmentation importante de la variété de sources de données à partir de laquelle vous pouvez importer à partir de. Cela est dû à l’introduction de la moderne obtenir des données de requête de données et importer les fonctionnalités dans SSDT en utilisant le langage de formule de requête de M.   

  Les solutions multidimensionnelles peuvent importer des données à partir de sources de données relationnelles à l’aide de fournisseurs OLE DB natifs et gérés.  
  
 Pour afficher la liste des sources de données externes que vous pouvez importer dans chaque modèle, consultez les rubriques suivantes :  
  
-   [Sources de données prises en charge](../analysis-services/tabular-models/data-sources-supported-ssas-tabular.md)  

-   [Sources de données prises en charge &#40;SSAS - Multidimensionnel&#41;](../analysis-services/multidimensional-models/supported-data-sources-ssas-multidimensional.md)  
  

  
##  <a name="bkmk_lang"></a> Prise en charge des langages de requêtes et de script  
 Analysis Services inclut MDX, DMX, DAX, XML/A, ASSL et TMSL. La prise en charge de ces langages peut varier selon le type de modèle. Si les spécifications du langage de requête et de script sont un facteur important, consultez la liste suivante.  

-   Les bases de données model tabulaires prennent en charge les calculs DAX, les requêtes DAX et les requêtes MDX. Cela est vrai à tous les niveaux de compatibilité. Langages de script sont ASSL (via XMLA) pour les niveaux de compatibilité 1050-1103 et TMSL (via XMLA) pour le niveau de compatibilité 1200 et supérieur. 

-   Les classeurs PowerPivot utilisent DAX pour les calculs, et DAX ou MDX pour les requêtes.  
  
-   Bases de données model multidimensionnelles prennent en charge les calculs MDX, les requêtes MDX, les requêtes DAX et ASSL. 
  
-   Les modèles d'exploration de données prennent en charge DMX et ASSL.  
  
-   PowerShell Analysis Services est pris en charge pour les modèles et les bases de données tabulaires et multidimensionnels.  
  
 Toutes les bases de données prennent en charge XML/A. Pour plus d’informations, consultez [Référence du langage des requêtes et expressions &#40;Analysis Services&#41;](http://msdn.microsoft.com/library/9597533d-35f4-4742-9d8c-7af392163527) et [Guide du développeur (Analysis Services)](../analysis-services/analysis-services-developer-documentation.md).  
  
##  <a name="bkmk_sec"></a> Fonctionnalités de sécurité  
 Toutes les solutions Analysis Services peuvent être sécurisées au niveau de la base de données. Les options de sécurité plus granulaires varient en fonction du mode. Si les paramètres de sécurité granulaires constituent un facteur important de votre solution, consultez la liste suivante pour vérifier que le niveau de sécurité que vous souhaitez est pris en charge dans le type de solution que vous voulez créer :  

  
-   Bases de données model tabulaire peuvent utiliser la sécurité au niveau des lignes, à l’aide d’autorisations basées sur le rôle.  
  
-   Bases de données model multidimensionnelles peuvent utiliser dimension et sécurité au niveau des cellules, à l’aide d’autorisations basées sur le rôle.  

-   Les classeurs[!INCLUDE[ssGemini](../includes/ssgemini-md.md)] sont sécurisés au niveau des fichiers à l’aide d’autorisations SharePoint.  
  
 Les classeurs[!INCLUDE[ssGemini](../includes/ssgemini-md.md)] peuvent être restaurés sur un serveur en mode tabulaire. Une fois que le fichier est restauré, il est découplé de SharePoint, ce qui vous permet d’utiliser toutes les fonctionnalités de modélisation tabulaire, y compris la sécurité de niveau ligne.  
  
##  <a name="bkmk_designer"></a> Outils de conception  
 Les compétences et l'expertise technique concernant la modélisation des données peuvent varier considérablement suivant les utilisateurs qui sont chargés de créer des modèles analytiques. Si la connaissance des outils ou le savoir-faire des utilisateurs constituent un facteur important, comparez les expériences suivantes pour la création de modèles.  
  
|Outil de modélisation|Mode d'utilisation|  
|-------------------|--------------|  
|[!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)]|Permet de créer tabulaire, multidimensionnel et les solutions d’exploration de données. Cet environnement de création utilise le shell Visual Studio pour fournir des espaces de travail, des volets de propriétés et la navigation entre les objets. Les utilisateurs techniques qui utilisent déjà Visual Studio préfèreront très probablement cet outil pour créer des applications décisionnelles.|  
|[!INCLUDE[ssGemini](../includes/ssgemini-md.md)] pour Excel|À utiliser pour créer un classeur [!INCLUDE[ssGemini](../includes/ssgemini-md.md)] que vous déployez ultérieurement sur une batterie de serveurs SharePoint disposant d’une installation de [!INCLUDE[ssGemini](../includes/ssgemini-md.md)] pour SharePoint. [!INCLUDE[ssGemini](../includes/ssgemini-md.md)] pour Excel offre un espace de travail d’application séparé qui s’ouvre sur Excel. Il utilise les mêmes métaphores visuelles (pages à onglets, disposition sous forme de grille et barre de formule) qu'Excel. Les utilisateurs qui maîtrisent parfaitement Excel préféreront cet outil sur [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)].|  
  
##  <a name="bkmk_client"></a> Prise en charge des applications clientes  
 Solutions en général, tabulaires et multidimensionnelles prennent en charge les applications clientes à l’aide d’un ou plusieurs des bibliothèques clientes Analysis Services (MSOLAP, AMOMD, ADOMD). Par exemple, Excel, Power BI Desktop et les applications personnalisées.   
 
 Si vous utilisez Reporting Services, la disponibilité des fonctionnalités de rapport varie en fonction des éditions et des modes de serveur. Par conséquent, le type de rapport que vous voulez créer peut influencer le mode de serveur que vous choisissez d'installer.  
  
 [!INCLUDE[ssCrescent](../includes/sscrescent-md.md)], le nouvel outil de conception de rapports Reporting Services qui s’exécute dans SharePoint, est disponible sur un serveur de rapports qui est déployé dans une batterie de serveurs SharePoint 2010. Le seul type de source de données utilisable avec ce rapport est une base de données de modèle tabulaire Analysis Services ou un classeur [!INCLUDE[ssGemini](../includes/ssgemini-md.md)] . Cela signifie que vous devez avoir un serveur en mode tabulaire ou un serveur [!INCLUDE[ssGemini](../includes/ssgemini-md.md)] pour SharePoint pour héberger la source de données utilisée par ce type de rapport. Vous ne pouvez pas utiliser un modèle multidimensionnel comme source de données pour un rapport [!INCLUDE[ssCrescent](../includes/sscrescent-md.md)] . Vous devez créer une connexion du modèle sémantique BI [!INCLUDE[ssGemini](../includes/ssgemini-md.md)] ou d’une source de données partagée Reporting Services à utiliser comme source de données pour un rapport [!INCLUDE[ssCrescent](../includes/sscrescent-md.md)] .  
  
 Le Générateur de rapports et le Concepteur de rapports peuvent utiliser toute base de données Analysis Services, dont des classeurs [!INCLUDE[ssGemini](../includes/ssgemini-md.md)] hébergés sur [!INCLUDE[ssGemini](../includes/ssgemini-md.md)] pour SharePoint.  
  
 Les rapports de tableau croisé dynamique Excel sont pris en charge par toutes les bases de données Analysis Services. Les fonctionnalités Excel sont identiques, que vous utilisiez une base de données tabulaire, une base de données multidimensionnelle ou un classeur [!INCLUDE[ssGemini](../includes/ssgemini-md.md)] , bien que l’écriture différée ne soit prise en charge que pour les bases de données multidimensionnelles.  
 
  
  
## <a name="see-also"></a>Voir aussi  
 [Gestion d’instances Analysis Services](../analysis-services/instances/analysis-services-instance-management.md)   
 [Nouveautés d’Analysis Services](../analysis-services/what-s-new-in-analysis-services.md)     

  
  
