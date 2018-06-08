---
title: Référence du langage (TMSL) de script de modèle tabulaire | Documents Microsoft
ms.date: 05/08/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tmsl
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 713fecbb367ac4717604c14b6f23baab905a993f
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="tabular-model-scripting-language-tmsl-reference"></a>Référence du langage (TMSL) de script de modèle tabulaire
[!INCLUDE[ssas-appliesto-sqlas-aas](../includes/ssas-appliesto-sqlas-aas.md)]

  Écriture de scripts langage TMSL (Tabular Model) est la syntaxe de définition de modèle de commande et d’objet pour les bases de données de modèle tabulaire Analysis Services au niveau de compatibilité 1200 ou supérieur. TMSL communique à Analysis Services via le protocole XMLA, où le [XMLA. Exécutez](../analysis-services/xmla/xml-elements-methods-execute.md) méthode accepte à la fois basée sur JSON **instruction** scripts TMSL, ainsi que les scripts basés sur XML traditionnels dans [Analysis Services Scripting Language &#40;ASSL de XMLA&#41; ](../analysis-services/scripting/analysis-services-scripting-language-assl-for-xmla.md).  
  
 Les éléments clés de TMSL sont les suivantes :  
  
-   En fonction de modèle tabulaire sémantique des métadonnées tabulaires. Un modèle tabulaire est composé de tables, colonnes et relations. Définitions d’objet équivalent dans TMSL sont maintenant, logiquement, tables, colonnes, relations et ainsi de suite.  
  
     Un nouveau moteur de métadonnées prend en charge ces définitions.  
  
-   Définitions d’objet sont structurées comme JSON au lieu de XML.  
  
 À l’exception de la façon dont la charge utile est au format (JSON ou XML), TMSL et ASSL sont fonctionnellement équivalents comment ils fournissent des commandes et des métadonnées aux méthodes XMLA utilisée pour le transfert de communications et les données du serveur.  
  
## <a name="how-to-use-tmsl"></a>L’utilisation de TMSL  
 Le moyen le plus simple pour Explorer l’écriture de scripts TMSL utilise les commandes CREATE, ALTER, DELETE ou processus dans SQL Server Management Studio (SSMS) sur un modèle que vous connaissez déjà. En supposant que vous utilisez un modèle existant, n’oubliez pas mettre à niveau au niveau de compatibilité 1200 ou supérieur.  
  
1.  Pour trouver la commande que vous souhaitez utiliser : [commandes dans le langage de script de modèle tabulaire &#40;TMSL&#41;](../analysis-services/tabular-models-scripting-language-commands/tmsl-reference-commands.md)  
  
2.  Vérifier la référence de définition d’objet pour les objets utilisés dans la commande : [définitions d’objets dans le langage de script de modèle tabulaire &#40;TMSL&#41;](../analysis-services/tabular-models-scripting-language-objects/tmsl-reference-tabular-objects.md)  
  
3.  Choisissez une méthode d’envoi d’un script TMSL :  
  
    -   Fenêtre XMLA dans Management Studio  
  
    -   **Invoke-ascmd** via PowerShell AMO ([applet de commande Invoke-ASCmd](../analysis-services/powershell/invoke-ascmd-cmdlet.md))  
  
    -   [Analysis Services tâche DDL d’exécution](../integration-services/control-flow/analysis-services-execute-ddl-task.md) dans SSIS.  
  
## <a name="model-definition-schema"></a>Schéma de définition du modèle  
 La capture d’écran suivante montre une version abrégée du schéma, réduite pour afficher les objets principaux.  
  
 ![SSAS_TabularMetadata](../analysis-services/media/ssas-tabularmetadata.JPG "SSAS_TabularMetadata")  
  
## <a name="scripting-languages-in-analysis-services"></a>Langages de script dans Analysis Services  
 Analysis Services prend en charge les langages de script ASSL et TMSL. Seuls les modèles tabulaires créés au niveau de compatibilité 1200 ou supérieur sont décrits dans TMS au format JSON.  
  
 [Analysis Services Scripting Language &#40;ASSL de XMLA&#41; ](../analysis-services/scripting/analysis-services-scripting-language-assl-for-xmla.md) a été le premier langage de script, et est toujours le langage de script uniquement pour les modèles multidimensionnels et tabulaires aux niveaux de compatibilité inférieurs (1100 ou 1103). Dans ASSL, les modèles tabulaires 110 x sont décrits en termes de multidimensionnelles, telles que **cube** (pour un modèle) et **measuregroup** (pour une table).  
  
> [!NOTE]  
>  Dans [SQL Server Data Tools (SSDT), vous pouvez mettre à niveau un modèle tabulaire de version antérieur pour utiliser TMSL en passant des sa **CompatibilityLevel** supérieure ou égale à 1200. N’oubliez pas de cette mise à niveau est irréversible. Avant la mise à niveau, sauvegardez votre modèle en cas de besoin plus tard la version d’origine.  
  
 Le tableau suivant présente la matrice de langage de script pour les modèles de données Analysis Services sur les différentes versions, niveaux de compatibilité spécifique.  

||||||  
|-|-|-|-|-|  
|**Version**|**Multidimensionnel**|**X 110 tabulaires**|**Tabulaires 1200**| **1400 tabulaire** |
|Azure Analysis Services|N/A|N/A|TMSL|TMSL| 
|SQL Server 2017|ASSL|ASSL|TMSL|TMSL| 
|SQL Server 2016|ASSL|ASSL|TMSL|TMSL| 
|SQL Server 2014|ASSL|ASSL|N/A|N/A|   
|SQL Server 2012|ASSL|ASSL|N/A|N/A|  

  
## <a name="see-also"></a>Voir aussi  
 [Niveau de compatibilité pour les modèles tabulaires dans Analysis Services](../analysis-services/tabular-models/compatibility-level-for-tabular-models-in-analysis-services.md)   
 [Analysis Services Scripting Language &#40;ASSL de XMLA&#41;](../analysis-services/scripting/analysis-services-scripting-language-assl-for-xmla.md)   
 [Déterminer le mode serveur d’une instance Analysis Services](../analysis-services/instances/determine-the-server-mode-of-an-analysis-services-instance.md)  
  
  
