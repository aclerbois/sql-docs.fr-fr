---
title: Extensions de personnalisation de Analysis Services | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: olap
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 85d4761b228a8b79e6c90ca0aa7f0c9a32738a03
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="analysis-services-personalization-extensions"></a>Extensions de personnalisation Analysis Services
[!INCLUDE[ssas-appliesto-sqlas](../../../includes/ssas-appliesto-sqlas.md)]
  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] extensions de personnalisation sont le fondement de l’idée d’implémentation d’une architecture de plug-in. Dans une architecture de plug-in, vous pouvez développer dynamiquement des nouveaux objets de cube et de nouvelles fonctionnalités, et les partager facilement avec d'autres développeurs. Par conséquent, [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] extensions de personnalisation fournissent des fonctionnalités qui permettent d’atteindre les objectifs suivants :  
  
-   **Conception et déploiement dynamiques** immédiatement après avoir conçu et déployer [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] extensions de personnalisation, les utilisateurs ont accès aux objets et fonctionnalités au début de la session utilisateur suivante.  
  
-   **Indépendance d’interface** , quelle que soit l’interface qui vous permet de créer le [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] extensions de personnalisation, les utilisateurs peuvent utiliser n’importe quelle interface pour accéder aux objets et fonctionnalités.  
  
-   **Contexte de session** [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] extensions de personnalisation ne sont pas des objets permanents dans l’infrastructure existante et ne nécessitent pas de cube pour être retraité. Elles sont exposées et créées pour l'utilisateur au moment où il se connecte à la base de données, et restent disponibles pour toute la durée de cette session utilisateur.  
  
-   **Distribution rapide** partage [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] avec d’autres développeurs de logiciels sans avoir à passer dans les spécifications détaillées décrivant où ou comment trouver les extensions de personnalisation des fonctionnalités étendues.  
  
 Les extensions de personnalisation [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] ont de nombreuses utilisations. Par exemple, votre société effectue des ventes qui impliquent des devises différentes. Vous créez un membre calculé qui retourne les ventes consolidées dans la devise locale de la personne qui accède au cube. Vous créez ce membre en tant qu'extension de personnalisation. Vous partagez alors ce membre calculé avec un groupe d'utilisateurs. Une fois le membre calculé partagé, ces utilisateurs ont un accès immédiat à celui-ci dès qu'ils se connectent au serveur. Ils bénéficient de cet accès même s'ils n'utilisent pas la même interface que celle utilisée pour créer le membre calculé.  
  
 [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] extensions de personnalisation sont une modification simple et élégante de l’architecture d’assembly managée existante et sont exposées partout dans le [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] <xref:Microsoft.AnalysisServices.AdomdServer> de l’objet modèle, la syntaxe de MDX (Multidimensional Expressions) et ensembles de lignes de schéma.  
  
## <a name="logical-architecture"></a>Architecture logique  
 L'architecture des extensions de personnalisation [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] est basée sur l'architecture d'assembly managée et les quatre éléments de base suivants :  
  
 L'attribut personnalisé [PlugInAttribute]  
 Lors du démarrage du service, [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] charge les assemblys requis et détermine les classes qui ont le <xref:Microsoft.AnalysisServices.AdomdServer.PlugInAttribute> attribut personnalisé.  
  
> [!NOTE]  
>  Le [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)] définit des attributs personnalisés comme une façon de décrire votre code et affecter le comportement à l'exécution. Pour plus d’informations, consultez la rubrique «[vue d’ensemble des attributs](http://go.microsoft.com/fwlink/?LinkId=82929), » dans le [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)] Guide du développeur sur MSDN.  
  
 Pour toutes les classes avec le <xref:Microsoft.AnalysisServices.AdomdServer.PlugInAttribute> attribut personnalisé, [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] appelle leurs constructeurs par défaut. Appel de tous les constructeurs au démarrage fournit un emplacement commun à partir de laquelle générer de nouveaux objets et qui est indépendant de toute activité de l’utilisateur.  
  
 En plus de la génération d'un petit cache d'informations sur la création et la gestion d'extensions de personnalisation, le constructeur de classe s'abonne généralement aux événements <xref:Microsoft.AnalysisServices.AdomdServer.Server.SessionOpened> et <xref:Microsoft.AnalysisServices.AdomdServer.Server.SessionClosing>. S'il ne s'abonne pas à ces événements, la classe peut être marquée de manière appropriée en vue d'être nettoyée par le garbage collector du CLR (Common Language Runtime).  
  
 Contexte de session  
 Pour les objets qui sont basés sur les extensions de personnalisation, [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] crée un environnement d'exécution pendant la session cliente et génère dynamiquement la plupart de ces objets dans cet environnement. Comme tout autre assembly CLR, cet environnement d'exécution a également accès aux autres fonctions et procédures stockées. Lors de la fin de la session utilisateur [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] supprime les objets créés dynamiquement et ferme l’environnement d’exécution.  
  
 Événements  
 Création d’un objet est déclenchée par les événements de session **OpenedCubeOpened Cube** et **ClosingCubeClosing Cube**.  
  
 La communication entre le client et le serveur se produit par le biais d'événements spécifiques. Ces événements indiquent au client les situations qui conduisent à la génération des objets du client. L'environnement du client est créé dynamiquement à l'aide de deux ensembles d'événements : événements de session et événements de cube.  
  
 Les événements de session sont associés à l'objet de serveur. Lorsqu’un client se connecte à un serveur, [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] crée une session et déclenche le <xref:Microsoft.AnalysisServices.AdomdServer.Server.SessionOpened> événement. Lorsqu’un client termine la session sur le serveur, [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] déclencheurs le <xref:Microsoft.AnalysisServices.AdomdServer.Server.SessionClosing> événement.  
  
 Les événements de cube sont associés à l'objet de connexion. La connexion à un cube déclenche l'événement <xref:Microsoft.AnalysisServices.AdomdServer.AdomdConnection.CubeOpened>. La fermeture de la connexion à un cube, en fermant le cube ou en passant à un autre cube, déclenche un événement <xref:Microsoft.AnalysisServices.AdomdServer.AdomdConnection.CubeClosing>.  
  
 Traçabilité et gestion des erreurs  
 Toute l'activité peut être tracée à l'aide de [!INCLUDE[ssSqlProfiler](../../../includes/sssqlprofiler-md.md)]. Les erreurs non gérées sont consignées dans le journal des événements Windows.  
  
 La création et la gestion de tous les objets sont indépendantes de cette architecture. Elles sont sous l'entière responsabilité des développeurs des objets.  
  
## <a name="infrastructure-foundations"></a>Base de l'infrastructure  
 Les extensions de personnalisation [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] sont basées sur des composants existants. Voici un récapitulatif des améliorations qui fournissent les fonctionnalités des extensions de personnalisation.  
  
### <a name="assemblies"></a>Assemblys  
 L'attribut personnalisé <xref:Microsoft.AnalysisServices.AdomdServer.PlugInAttribute> peut être ajouté à vos assemblys personnalisés pour identifier les classes des extensions de personnalisation [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)].  
  
### <a name="changes-to-the-adomdserver-object-model"></a>Modifications apportées au modèle objet AdomdServer  
 Les objets suivants du modèle objet <xref:Microsoft.AnalysisServices.AdomdServer> ont été améliorés ou ont été ajoutés au modèle.  
  
#### <a name="new-adomdconnection-class"></a>Nouvelle classe AdomdConnection  
 La nouvelle classe <xref:Microsoft.AnalysisServices.AdomdServer.AdomdConnection> expose plusieurs extensions de personnalisation par le biais de propriétés et d'événements.  
  
 **Propriétés**  
  
-   <xref:Microsoft.AnalysisServices.AdomdServer.AdomdConnection.SessionID%2A>, une valeur de chaîne en lecture seule qui représente l'ID de session de la connexion actuelle.  
  
-   <xref:Microsoft.AnalysisServices.AdomdServer.AdomdConnection.ClientCulture%2A>, une référence en lecture seule à la culture du client associée à la session active.  
  
-   <xref:Microsoft.AnalysisServices.AdomdServer.AdomdConnection.User%2A>, une référence en lecture seule à l'interface d'identité représentant l'utilisateur actuel.  
  
 **Événements**  
  
-   <xref:Microsoft.AnalysisServices.AdomdServer.AdomdConnection.CubeOpened>  
  
-   <xref:Microsoft.AnalysisServices.AdomdServer.AdomdConnection.CubeClosing>  
  
#### <a name="new-properties-in-the-context-class"></a>Nouvelles propriétés de la classe Context  
 La classe <xref:Microsoft.AnalysisServices.AdomdServer.Context> dispose de deux nouvelles propriétés :  
  
-   <xref:Microsoft.AnalysisServices.AdomdServer.Context.Server%2A>, une référence en lecture seule au nouvel objet de serveur.  
  
-   <xref:Microsoft.AnalysisServices.AdomdServer.Context.CurrentConnection%2A>, une référence en lecture seule au nouvel objet <xref:Microsoft.AnalysisServices.AdomdServer.AdomdConnection>.  
  
#### <a name="new-server-class"></a>Nouvelle classe Server  
 La nouvelle classe <xref:Microsoft.AnalysisServices.AdomdServer.Server> expose plusieurs extensions de personnalisation par le biais d'événements et de propriétés de classe.  
  
 **Propriétés**  
  
-   <xref:Microsoft.AnalysisServices.AdomdServer.Server.Name%2A>, une valeur de chaîne en lecture seule représentant le nom du serveur.  
  
-   <xref:Microsoft.AnalysisServices.AdomdServer.Server.Culture%2A>, une référence en lecture seule à la culture globale associée au serveur.  
  
 **Événements**  
  
-   <xref:Microsoft.AnalysisServices.AdomdServer.Server.SessionOpened>  
  
-   <xref:Microsoft.AnalysisServices.AdomdServer.Server.SessionClosing>  
  
#### <a name="adomdcommand-class"></a>Classe AdomdCommand  
 La classe <xref:Microsoft.AnalysisServices.AdomdServer.AdomdCommand> prend maintenant en charge les commandes MDX suivantes :  
  
-   [CRÉER une instruction MEMBER & #40 ; MDX & #41 ;](../../../mdx/mdx-data-definition-create-member.md)  
  
-   [Instruction de mise à jour MEMBER &#40;MDX&#41;](../../../mdx/mdx-data-definition-update-member.md)  
  
-   [Instruction de membre DROP &#40;MDX&#41;](../../../mdx/mdx-data-definition-drop-member.md)  
  
-   [CRÉER une instruction SET & #40 ; MDX & #41 ;](../../../mdx/mdx-data-definition-create-set.md)  
  
-   [Instruction SET de dépôt &#40;MDX&#41;](../../../mdx/mdx-data-definition-drop-set.md)  
  
-   [Instruction CREATE KPI &#40;MDX&#41;](../../../mdx/mdx-data-definition-create-kpi.md)  
  
-   [Instruction d’indicateur de performance clé DROP &#40;MDX&#41;](../../../mdx/mdx-data-definition-drop-kpi.md)  
  
### <a name="mdx-extensions-and-enhancements"></a>Extensions MDX et améliorations  
 La commande CREATE MEMBER est améliorée avec les **légende** propriété, le **display_folder** propriété et le **associated_measure_group** propriété.  
  
 La commande UPDATE MEMBER est ajoutée pour éviter la récréation du membre lorsqu'une mise à jour est nécessaire avec la perte de priorité conséquente pour la résolution des calculs. Les mises à jour ne peut pas modifier l’étendue du membre calculé, déplacer le membre calculé à un parent différent, ou définir une autre **solveorder**.  
  
 La commande CREATE SET est améliorée avec les **légende** propriété, le **display_folder** propriété et la nouvelle **statique | DYNAMIQUE** (mot clé). *Statique* signifie que le jeu est évalué uniquement au moment de la création. *Dynamique* signifie que le jeu est évalué chaque fois que le jeu est utilisé dans une requête. La valeur par défaut est **statique** si un mot clé est omis.  
  
 Les commandes CREATE KPI et DROP KPI sont ajoutées à la syntaxe MDX. Les indicateurs de performance clés peuvent être créés dynamiquement à partir de tout script MDX.  
  
### <a name="schema-rowsets-extensions"></a>Extensions d'ensembles de lignes de schéma  
 Sur MDSCHEMA_MEMBERS *étendue* colonne est ajoutée. Les valeurs de portée sont les suivantes : MDMEMBER_SCOPE_GLOBAL=1, MDMEMBER_SCOPE_SESSION=2.  
  
 Sur MDSCHEMA_SETS *set_evaluation_context* colonne est ajoutée. Les valeurs de contexte d'évaluation du jeu sont les suivantes : MDSET_RESOLUTION_STATIC = 1, MDSET_RESOLUTION_DYNAMIC = 2.  
  
 Sur MDSCHEMA_KPIS, la colonne de portée est ajoutée. Les valeurs de portée sont les suivantes : MDKPI_SCOPE_GLOBAL=1, MDKPI_SCOPE_SESSION=2.  
  
  
