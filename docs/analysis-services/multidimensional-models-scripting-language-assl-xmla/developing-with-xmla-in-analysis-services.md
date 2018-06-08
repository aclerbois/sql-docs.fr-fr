---
title: Développement avec XMLA dans Analysis Services | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: xmla
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 5a7af006d5d52002480844b8776d8262fa8ae202
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="developing-with-xmla-in-analysis-services"></a>Développement avec XMLA dans Analysis Services
  XMLA (XML for Analysis) est un protocole XML basé sur SOAP (Simple Object Access Protocol) conçu spécifiquement pour offrir un accès universel à n'importe quelle source de données multidimensionnelle standard accessible via une connexion HTTP. [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] utilise XMLA comme seul protocole pour communiquer avec les applications clientes. Fondamentalement, toutes les bibliothèques clientes prises en charge par Analysis Services formulent des demandes et des réponses XMLA.  
  
 En tant que développeur, vous pouvez utiliser XMLA pour intégrer une application cliente à [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], sans aucune dépendance du .NET Framework ou des interfaces COM. La configuration requise pour une application qui inclut l'hébergement sur un large éventail de plateformes peut être satisfaite à l'aide de XMLA et d'une connexion HTTP à [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].  
  
 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] est entièrement conforme à la spécification 1.1 XMLA, mais l'étend également pour permettre la définition de données, la manipulation des données et la prise en charge du contrôle de données. Les extensions Analysis Services sont désignées par le terme ASSL (Analysis Services Scripting Language). L'utilisation de XMLA et d'ASSL autorise un plus large ensemble de fonctionnalités que celui fournit par XMLA. Pour plus d’informations sur ASSL, consultez [développement avec Analysis Services Scripting Language &#40;ASSL&#41;](../../analysis-services/multidimensional-models/scripting-language-assl/developing-with-analysis-services-scripting-language-assl.md).  
  
## <a name="in-this-section"></a>Dans cette section  
  
|Rubrique| Description|  
|-----------|-----------------|  
|[Gestion des connexions et Sessions &#40;XMLA&#41;](../../analysis-services/multidimensional-models-scripting-language-assl-xmla/managing-connections-and-sessions-xmla.md)|Explique comment se connecter à une instance [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] et comment gérer les sessions et la conservation de l'état avec XMLA.|  
|[Gestion des erreurs et avertissements &#40;XMLA&#41;](../../analysis-services/multidimensional-models-scripting-language-assl-xmla/handling-errors-and-warnings-xmla.md)|Indique comment [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] retourne les informations d'erreur et d'avertissement pour les méthodes et les commandes XMLA.|  
|[Définition et identification d’objets &#40;XMLA&#41;](../../analysis-services/multidimensional-models-scripting-language-assl-xmla/defining-and-identifying-objects-xmla.md)|Décrit les identificateurs et références d'objet et explique comment les utiliser dans les commandes XMLA.|  
|[Gestion des Transactions &#40;XMLA&#41;](../../analysis-services/multidimensional-models-scripting-language-assl-xmla/managing-transactions-xmla.md)|Explique comment utiliser le [BeginTransaction](../../analysis-services/xmla/xml-elements-commands/begintransaction-element-xmla.md), [CommitTransaction](../../analysis-services/xmla/xml-elements-commands/committransaction-element-xmla.md), et [RollbackTransaction](../../analysis-services/xmla/xml-elements-commands/rollbacktransaction-element-xmla.md) commandes pour définir explicitement et gérer une transaction sur la session XMLA active.|  
|[Annulation de commandes &#40;XMLA&#41;](../../analysis-services/multidimensional-models-scripting-language-assl-xmla/canceling-commands-xmla.md)|Décrit comment utiliser le [Annuler](../../analysis-services/xmla/xml-elements-commands/cancel-element-xmla.md)commande pour annuler des commandes, les sessions et connexions XMLA.|  
|[Exécution d’opérations de traitement par lots &#40;XMLA&#41;](../../analysis-services/multidimensional-models-scripting-language-assl-xmla/performing-batch-operations-xmla.md)|Décrit comment utiliser le [lot](../../analysis-services/xmla/xml-elements-commands/batch-element-xmla.md) commande à exécuter XMLA plusieurs commandes, en série ou en parallèle, dans la même transaction ou en tant que transactions distinctes, à l’aide d’un seul XMLA [Execute](../../analysis-services/xmla/xml-elements-methods-execute.md) (méthode).|  
|[Création et modification d’objets &#40;XMLA&#41;](../../analysis-services/multidimensional-models-scripting-language-assl-xmla/creating-and-altering-objects-xmla.md)|Décrit comment utiliser le [créer](../../analysis-services/xmla/xml-elements-commands/create-element-xmla.md), [Alter](../../analysis-services/xmla/xml-elements-commands/alter-element-xmla.md), et [supprimer](../../analysis-services/xmla/xml-elements-commands/delete-element-xmla.md) commandes, ainsi que les éléments d’Analysis Services Scripting Language (ASSL), pour définir, modifier ou supprimer des objets d’un [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] instance.|  
|[Verrouillage et déverrouillage de bases de données &#40;XMLA&#41;](../../analysis-services/multidimensional-models-scripting-language-assl-xmla/locking-and-unlocking-databases-xmla.md)|Explique comment utiliser le [verrou](../../analysis-services/xmla/xml-elements-commands/lock-element-xmla.md) et [Unlock](../../analysis-services/xmla/xml-elements-commands/unlock-element-xmla.md) commandes pour verrouiller et déverrouiller une [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] base de données.|  
|[Le traitement des objets & #40 ; XMLA & #41 ;](../../analysis-services/multidimensional-models-scripting-language-assl-xmla/processing-objects-xmla.md)|Décrit comment utiliser le [processus](../../analysis-services/xmla/xml-elements-commands/process-element-xmla.md) commande au processus un [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] objet.|  
|[La fusion de Partitions &#40;XMLA&#41;](../../analysis-services/multidimensional-models-scripting-language-assl-xmla/merging-partitions-xmla.md)|Décrit comment utiliser le [MergePartitions](../../analysis-services/xmla/xml-elements-commands/mergepartitions-element-xmla.md) commande pour fusionner des partitions sur un [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] instance.|  
|[Conception d’agrégations &#40;XMLA&#41;](../../analysis-services/multidimensional-models-scripting-language-assl-xmla/designing-aggregations-xmla.md)|Décrit comment utiliser le [DesignAggregations](../../analysis-services/xmla/xml-elements-commands/designaggregations-element-xmla.md) commande, soit dans itératif ou en mode de traitement par lots, pour concevoir des agrégations pour une conception d’agrégation dans [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)].|  
|[Sauvegarde, restauration et synchronisation de bases de données & #40 ; XMLA & #41 ;](../../analysis-services/multidimensional-models-scripting-language-assl-xmla/backing-up-restoring-and-synchronizing-databases-xmla.md)|Décrit comment utiliser le [sauvegarde](../../analysis-services/xmla/xml-elements-commands/backup-element-xmla.md) et [restaurer](../../analysis-services/xmla/xml-elements-commands/restore-element-xmla.md) commandes pour sauvegarder et restaurer un [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] base de données à partir d’un fichier de sauvegarde.<br /><br /> Décrit également comment utiliser le [synchroniser](../../analysis-services/xmla/xml-elements-commands/synchronize-element-xmla.md) commande pour synchroniser un [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] base de données avec une base de données existante sur la même instance ou sur une autre instance.|  
|[Insertion, mise à jour et suppression de membres &#40;XMLA&#41;](../../analysis-services/multidimensional-models-scripting-language-assl-xmla/inserting-updating-and-dropping-members-xmla.md)|Décrit comment utiliser le [insérer](../../analysis-services/xmla/xml-elements-commands/insert-element-xmla.md), [mise à jour](../../analysis-services/xmla/xml-elements-commands/update-element-xmla.md), et [supprimer](../../analysis-services/xmla/xml-elements-commands/drop-element-xmla.md) commandes permettant d’ajouter, modifier ou supprimer des membres dans une dimension activée en écriture.|  
|[Mise à jour de cellules &#40;XMLA&#41;](../../analysis-services/multidimensional-models-scripting-language-assl-xmla/updating-cells-xmla.md)|Décrit comment utiliser le [UpdateCells](../../analysis-services/xmla/xml-elements-commands/updatecells-element-xmla.md) commande pour modifier les valeurs des cellules dans une partition activée en écriture.|  
|[Gestion des Caches &#40;XMLA&#41;](../../analysis-services/multidimensional-models-scripting-language-assl-xmla/managing-caches-xmla.md)|Explique comment utiliser le [ClearCache](../../analysis-services/xmla/xml-elements-commands/clearcache-element-xmla.md) commande pour effacer les caches de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] objets.|  
|[Surveillance de Traces &#40;XMLA&#41;](../../analysis-services/multidimensional-models-scripting-language-assl-xmla/monitoring-traces-xmla.md)|Décrit comment utiliser le [s’abonner](../../analysis-services/xmla/xml-elements-commands/subscribe-element-xmla.md) commande s’abonner à et surveiller une trace existante sur un [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] instance.|  
  
## <a name="data-mining-with-xmla"></a>Exploration de données avec XMLA  
 XML for Analysis prend entièrement en charge les ensembles de lignes de schéma d'exploration de données. Ces ensembles de lignes fournissent des informations permettant d’interroger des modèles d’exploration de données à l’aide de la [Discover](../../analysis-services/xmla/xml-elements-methods-discover.md) (méthode). Pour plus d’informations sur les ensembles de lignes de schéma de données d’exploration de données, consultez [Data Mining Schema Rowsets](../../analysis-services/schema-rowsets/data-mining/data-mining-schema-rowsets.md)  
  
 Pour plus d’informations sur DMX, consultez [Data Mining Extensions &#40;DMX&#41; référence](../../dmx/data-mining-extensions-dmx-reference.md).  
  
## <a name="namespace-and-schema"></a>Espace de noms et schéma  
  
### <a name="namespace"></a>Espace de noms  
 Le schéma défini dans cette spécification utilise l’espace de noms XML `http://schemas.microsoft.com/AnalysisServices/2003/Engine` et l’abréviation standard « DDL ».  
  
### <a name="schema"></a>Schéma  
 La définition d'un schéma XSD (XML Schema definition language) pour le langage de définition d'objet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] repose sur la définition des éléments et de la hiérarchie de schéma de cette section.  
  
## <a name="extensibility"></a>Extensibilité  
 Extensibilité du schéma de langage de définition objet est fournie au moyen d’un **Annotation** élément qui est inclus dans tous les objets. Cet élément peut contenir du code XML valide issu d'un espace de noms XML (différent de l'espace de noms cible qui définit le langage de définition de données), soumis aux règles suivantes :  
  
-   Les données XML peuvent contenir uniquement des éléments.  
  
-   Chaque élément doit avoir un nom unique. Il est recommandé que la valeur de **nom** référencer l’espace de noms cible.  
  
 Ces règles sont imposées afin que le contenu de la **Annotation** balise peut être exposée comme un ensemble de paires de nom/valeur via les objets DSO (Decision Support) 9.0.  
  
 Les commentaires et l’espace blanc dans le **Annotation** balise qui ne sont pas compris dans un élément enfant ne soit pas conservé. De plus, tous les éléments doivent être accessibles en lecture-écriture ; les éléments en lecture seule sont ignorés.  
  
 Le schéma du langage de définition d'objet est figé dans le sens où le serveur ne permet pas la substitution des éléments définis dans le schéma par des types dérivés. Par conséquent, le serveur n'accepte que l'ensemble d'éléments définis ici et pas les autres éléments ou attributs. En présence d'éléments inconnus, le moteur [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] génère une erreur.  
  
## <a name="see-also"></a>Voir aussi  
 [Développement avec Analysis Services Scripting Language &#40;ASSL&#41;](../../analysis-services/multidimensional-models/scripting-language-assl/developing-with-analysis-services-scripting-language-assl.md)   
 [Présentation de l’architecture Microsoft OLAP](../../analysis-services/multidimensional-models/olap-physical/understanding-microsoft-olap-architecture.md)  
  
  
