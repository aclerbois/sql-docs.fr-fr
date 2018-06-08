---
title: Déployer des Solutions de modèle avec l’utilitaire de déploiement | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 76d1a3e3cfff777f610bb00f52644af3903ac615
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="deploy-model-solutions-with-the-deployment-utility"></a>Déployer des solutions de modèle avec l'utilitaire de déploiement
[!INCLUDE[ssas-appliesto-sqlas-all-aas](../../includes/ssas-appliesto-sqlas-all-aas.md)]

  L'utilitaire **Microsoft.AnalysisServices.Deployment** permet de démarrer le moteur de déploiement de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] à partir de l'invite de commandes. Comme un fichier d'entrée, l'utilitaire emploie les fichiers de sorties XML générés par la construction d'un projet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]. Les fichiers d'entrées sont facilement modifiables pour personnaliser le déploiement d'un projet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] . Le script de déploiement généré peut alors être immédiatement exécuté ou enregistré en vue d'un déploiement ultérieur.  
  
> [!NOTE]
> Le [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] Assistant/utilitaire de déploiement est installé avec [SQL Server Management Studio](../../ssms/download-sql-server-management-studio-ssms.md) (SSMS). Veillez à l’aide de la version la plus récente. Par défaut, la dernière version de l’Assistant de déploiement est installée vers C:\Program Files (x86) \Microsoft SQL Server\140\Tools\Binn\ManagementStudio. 

## <a name="syntax"></a>Syntaxe  
  
```  
  
Microsoft.AnalysisServices.Deployment [ASdatabasefile]   
    {[/s[:logfile]] | [/a] | [[/o[:output_script_file]] [/d]]}  
```  
  
##  <a name="Arguments"></a> Arguments  
 *ASdatabasefile*  
 Chemin complet du dossier dans lequel le fichier de script de déploiement [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] (.asdatabase) se trouve. Ce fichier est généré lorsque vous déployez un projet dans [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)]. Il se trouve dans le dossier bin du projet. Le fichier de base de données contient les définitions d'objets à déployer. En l'absence de toute spécification, le dossier en cours est utilisé.  
  
 **/s**  
 Exécute l'utilitaire en mode silencieux et n'affiche pas de boîte de dialogue. Pour plus d'informations sur les modes, consultez la section, [Modes](#Modes), plus loin dans cette rubrique.  
  
 *logfile*  
 Chemin d'accès complet et nom de fichier du fichier journal. Les événements de trace sont consignés dans le fichier journal spécifié. Si le fichier journal existe déjà, le contenu du fichier est remplacé.  
  
 **/a**  
 Exécute l'utilitaire en mode réponse. Toutes les réponses effectuées pendant la partie Assistant de l'utilitaire sont réécrites dans les fichiers d'entrée, mais aucune modification n'est apportée aux cibles de déploiement.  
  
 **/o**  
 Exécute l'utilitaire en mode de sortie. Aucun déploiement n'est effectué, mais le script XML for Analysis (XMLA) qui serait normalement envoyé aux cibles de déploiement est plutôt enregistré dans le fichier de script de sortie spécifié. Si *output_script_file* n’est pas spécifié, l’utilitaire tente d’utiliser le fichier de script de sortie spécifié dans le fichier d’entrée des options de déploiement (.deploymentoptions). Si un fichier de script de sortie n'est pas spécifié dans le fichier d'entrée des options de déploiement, une erreur se produit.  
  
 Pour plus d'informations sur les modes, consultez la section, [Modes](#Modes), plus loin dans cette rubrique.  
  
 *output_script_file*  
 Chemin d'accès complet et nom de fichier du fichier de script de sortie.  
  
 **/d**  
 Si l’argument **/o** est utilisé, il spécifie que l’utilitaire ne doit pas se connecter à l’instance cible. Aucune connexion n'étant établie aux cibles de déploiement, le script de sortie est généré uniquement sur la base des informations récupérées à partir des fichiers d'entrée.  
  
> [!NOTE]  
>  L’argument **/d** est utilisé uniquement en mode de sortie. Cet argument est ignoré s'il est spécifié en mode réponse ou en mode silencieux. Pour plus d'informations sur les modes, consultez la section, [Modes](#Modes), plus loin dans cette rubrique.  
  
## <a name="remarks"></a>Notes  
 L'utilitaire **Microsoft.AnalysisServices.Deployment** prend un ensemble de fichiers qui fournit les définitions d'objets, les cibles de déploiement, les options de déploiement et les paramètres de configuration et tente de déployer les définitions d'objets dans les cibles de déploiement spécifiées, en utilisant les options de déploiement et les paramètres de configuration spécifiés. Cet utilitaire peut fournir une interface utilisateur lorsqu'il est invoqué dans un fichier de réponse ou en mode de sortie. Pour plus d’informations sur l’utilisation de l’interface utilisateur fournie pour cet utilitaire pour créer des fichiers de réponse, consultez [Déployer des solutions de modèles à l’aide de l’Assistant Déploiement](../../analysis-services/multidimensional-models/deploy-model-solutions-using-the-deployment-wizard.md).  
  
 L’utilitaire se trouve dans le dossier de \Microsoft SQL Server\140\Binn\ManagementStudio \Program files (x86).  
  
##  <a name="Modes"></a> Modes  
 L'utilitaire peut être employé dans les modes indiqués dans le tableau suivant.  
  
|Mode|Description|  
|----------|-----------------|  
|Mode silencieux|Aucune interface utilisateur n'est affichée et toutes les informations requises pour le déploiement sont fournies par les fichiers d'entrée. Aucune progression n'est affichée par l'utilitaire en mode silencieux. Un fichier journal facultatif est plutôt utilisé pour capturer les informations de progression et d'erreurs en vue d'une révision ultérieure.|  
|Mode réponse|L'interface utilisateur de l'Assistant Déploiement s'affiche et les réponses utilisateur sont enregistrées dans les fichiers d'entrée spécifiés en vue d'un déploiement ultérieur. Le déploiement ne s'effectue pas en mode réponse. Le mode réponse ne sert qu'à capturer les réponses de l'utilisateur.|  
|Mode de sortie|Aucune interface utilisateur n'est affichée et toutes les informations requises pour le déploiement sont fournies par les fichiers d'entrée.<br /><br /> Cependant, contrairement au mode silencieux, la sortie de l'utilitaire est écrite dans un fichier de script de sortie, elle n'est pas envoyée aux cibles de déploiement indiquées dans les fichiers d'entrée. Sauf si l’argument **/d** est spécifié, l’utilitaire se connecte à chaque cible de déploiement pour comparer les métadonnées lors de la génération du fichier de script de sortie.|  
  
 [Retour aux arguments](#Arguments)  
  
## <a name="examples"></a>Exemples  
 L'exemple suivant illustre le déploiement d'un projet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] en mode silencieux, consignant les messages de progression et d'erreur en vue d'une analyse ultérieure :  
  
 `Microsoft.AnalysisServices.Deployment.exe`  
  
 `<drive>:\My Documents\Visual Studio 2010\Projects\AdventureWorksProject\Project1\bin`  
  
 `/s: C:\ My Documents\Visual Studio 2010\Projects\AdventureWorksProject\Project1\bin\deployment.log`  
  
## <a name="see-also"></a>Voir aussi  
 [Référence de l’utilitaire d’invite de commandes &#40;moteur de base de données&#41;](../../tools/command-prompt-utility-reference-database-engine.md)  
  
  
