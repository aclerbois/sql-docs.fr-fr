---
title: Requête d’exploration de données, tâche | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: control-flow
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.dataminingquerytask.f1
- sql13.dts.designer.dmquerytask.miningmodel.f1
- sql13.dts.designer.dmquerytask.query.f1
- sql13.dts.designer.dmquerytask.output.f1
helpviewer_keywords:
- prediction queries [Integration Services]
- Data Mining Query task [Integration Services]
ms.assetid: f489348c-2008-4f66-8c2c-c07c3029439a
caps.latest.revision: 45
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: fb70afd25086bdee1c671f514cc5fc77cedc56d2
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="data-mining-query-task"></a>Data Mining Query Task
  La tâche de requête d'exploration de données exécute des requêtes de prédiction basées sur les modèles d'exploration de données intégrés à [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. La requête de prédiction crée une prédiction de nouvelles données à l'aide de modèles d'exploration de données. Par exemple, une requête de prédiction peut prédire le nombre de voiliers susceptibles d'être vendus pendant les mois d'été ou générer la liste des prospects susceptibles d'acheter un voilier.  
  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] met à votre disposition des tâches qui effectuent d’autres opérations Business Intelligence, telles que l’exécution d’instructions DDL (Data Definition Language) et le traitement d’objets analytiques.  
  
 Pour plus d'informations sur les autres tâches Business Intelligence, cliquez sur l'une des rubriques suivantes :  
  
-   [Tâche DDL d’exécution de SQL Server Analysis Services](../../integration-services/control-flow/analysis-services-execute-ddl-task.md)  
  
-   [Tâche de traitement d'Analysis Services](../../integration-services/control-flow/analysis-services-processing-task.md)  
  
## <a name="prediction-queries"></a>Requêtes de prédiction  
 La requête est une instruction DMX (Data Mining Extensions). Le langage DMX est une extension du langage SQL qui prend en charge l'utilisation de modèles d'exploration de données. Pour plus d’informations sur l’utilisation du langage DMX, consultez [Guide de référence du langage DMX & #40;Data Mining Extensions&#41;](../../dmx/data-mining-extensions-dmx-reference.md).  
  
 La tâche peut interroger plusieurs modèles d'exploration de données basés sur la même structure d'exploration de données. Un modèle d'exploration de données est construit à partir de l'un des algorithmes d'exploration de données fournis par [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] . La structure d'exploration de données référencée par la tâche de requête d'exploration de données peut comprendre plusieurs modèles d'exploration de données, construits à partir de différents algorithmes. Pour plus d’informations, consultez [Structures d’exploration de données &#40;Analysis Services - exploration de données&#41;](../../analysis-services/data-mining/mining-structures-analysis-services-data-mining.md) et [Algorithmes d’exploration de données &#40;Analysis Services - Exploration de données&#41;](../../analysis-services/data-mining/data-mining-algorithms-analysis-services-data-mining.md).  
  
 La requête de prédiction exécutée par la tâche de requête d'exploration de données renvoie un résultat qui se présente sous la forme d'une seule ligne ou d'un ensemble de données. Une requête qui ne retourne qu'une ligne est appelée « requête singleton » : par exemple, la requête qui prédit le nombre de voiliers susceptibles d'être vendus pendant les mois d'été retourne un nombre. Pour plus d’informations sur les requêtes de prédiction qui renvoient une seule ligne, consultez [Outils de requête d’exploration de données](../../analysis-services/data-mining/data-mining-query-tools.md).  
  
 Les résultats des requêtes sont enregistrés dans des tables. Si une table portant le nom spécifié par la tâche de requête d'exploration de données existe déjà, la tâche peut créer une nouvelle table à partir du même nom, auquel elle ajoute un nombre, ou bien remplacer le contenu de la table.  
  
 Si le résultat comprend une imbrication, elle est mise à plat avant d'être enregistrée. La mise à plat d'un résultat change en table un ensemble de résultats imbriqué. Par exemple, la mise à plat d'un résultat imbriqué composé d'une colonne **Customer** et d'une colonne **Product** imbriquée ajoute des lignes à la colonne **Customer** afin de créer une table qui comprend les données de produit associées à chaque client. Ainsi, dans le cas d'un client auquel sont associés trois produits différents, l'opération génère une table de trois lignes, contenant chacune le nom du client et un nom de produit différent. Si le mot clé FLATTENED est omis, la table ne contient que la colonne **Customer** et une seule ligne par client. Pour plus d’informations, consultez [SELECT &#40;DMX&#41;](../../dmx/select-dmx.md).  
  
## <a name="configuration-of-the-data-mining-query-task"></a>Configuration de la tâche de requête d'exploration de données  
 La tâche de requête d'exploration de données requiert deux connexions. La première connexion est celle d'un gestionnaire de connexions [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] à une instance de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] ou à un projet [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] qui contient la structure et le modèle d'exploration de données. La seconde connexion est celle d'un gestionnaire de connexions OLE DB à la base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui contient la table dans laquelle la tâche écrit les données. Pour plus d'informations, consultez [Analysis Services Connection Manager](../../integration-services/connection-manager/analysis-services-connection-manager.md) et [OLE DB Connection Manager](../../integration-services/connection-manager/ole-db-connection-manager.md).  
  
 Vous pouvez définir les propriétés par le biais du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] ou par programmation.  
  
> [!NOTE]  
>  L'éditeur de tâche de requête d'exploration de données n'a pas de page Expressions. À la place, utilisez la fenêtre **Propriétés** pour accéder aux outils de création et de gestion des expressions des propriétés de la tâche de requête d'exploration de données.  
  
 Pour plus d'informations sur la définition de ces propriétés dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , cliquez sur la rubrique suivante :  
  
-   [Définir les propriétés d'une tâche ou d'un conteneur](http://msdn.microsoft.com/library/52d47ca4-fb8c-493d-8b2b-48bb269f859b)  
  
## <a name="programmatic-configuration-of-data-mining-query-task"></a>Configuration par programmation de la tâche de requête d'exploration de données  
 Pour plus d'informations sur la définition par programmation de ces propriétés, cliquez sur l'une des rubriques suivantes :  
  
-   <xref:Microsoft.SqlServer.Dts.Tasks.DMQueryTask.DMQueryTask>  
  
## <a name="data-mining-query-task-editor-mining-model-tab"></a>Éditeur de tâche de requête d'exploration de données (onglet Modèle d'exploration de données)
  Utilisez l’onglet **Modèle d’exploration de données** de la boîte de dialogue **Tâche de requête d’exploration de données** pour spécifier la structure et le modèle d’exploration de données à utiliser.  
  
 Pour en savoir plus sur l’implémentation de l’exploration de données dans les packages, consultez [Tâche de requête d’exploration de données](../../integration-services/control-flow/data-mining-query-task.md) et [Solutions d’exploration de données](../../analysis-services/data-mining/data-mining-solutions.md).  
  
### <a name="general-options"></a>Options générales  
 **Nom**  
 Fournissez un nom unique pour la tâche de requête d'exploration de données. Ce nom sert d'étiquette à l'icône de la tâche.  
  
> [!NOTE]  
>  Les noms de tâche doivent être uniques dans un package.  
  
 **Description**  
 Saisissez la description de la tâche de requête d'exploration de données.  
  
### <a name="mining-model-tab-options"></a>Options de l'onglet Modèle d'exploration de données  
 **Connexion**  
 Choisissez un gestionnaire de connexions [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] dans la liste ou cliquez sur **Nouveau** pour créer un gestionnaire de connexions.  
  
 **Rubriques connexes :**  [Gestionnaire de connexions Analysis Services](../../integration-services/connection-manager/analysis-services-connection-manager.md)  
  
 **Nouveau**  
 Créez un gestionnaire de connexions [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] .  
  
 **Rubriques connexes :** [Référence de l’interface utilisateur de la boîte de dialogue Ajout d’un gestionnaire de connexions Analysis Services](../../integration-services/connection-manager/add-analysis-services-connection-manager-dialog-box-ui-reference.md)  
  
 **Structure d'exploration de données**  
 Sélectionnez une structure d'exploration de données dans la liste.  
  
 **Modèles d'exploration de données**  
 Sélectionnez un modèle d'exploration de données qui repose sur la structure sélectionnée.  

## <a name="data-mining-query-task-editor-query-tab"></a>Éditeur de tâche de requête d'exploration de données (onglet Requête)
  Utilisez l’onglet **Requête** de la boîte de dialogue **Tâche de requête d’exploration de données** pour créer des requêtes de prédiction basées sur un modèle d’exploration de données. Dans cette boîte de dialogue, vous pouvez également lier des paramètres et des ensembles de résultats à des variables.  
  
 Pour en savoir plus sur l’implémentation de l’exploration de données dans les packages, consultez [Tâche de requête d’exploration de données](../../integration-services/control-flow/data-mining-query-task.md) et [Solutions d’exploration de données](../../analysis-services/data-mining/data-mining-solutions.md).  
  
### <a name="general-options"></a>Options générales  
 **Nom**  
 Fournissez un nom unique pour la tâche de requête d'exploration de données. Ce nom sert d'étiquette à l'icône de la tâche.  
  
> [!NOTE]  
>  Les noms de tâche doivent être uniques dans un package.  
  
 **Description**  
 Saisissez la description de la tâche de requête d'exploration de données.  
  
### <a name="build-query-tab-options"></a>Options de l'onglet Générer la requête  
 **Requête d'exploration de données**  
 Tapez une requête d'exploration de données.  
  
 **Rubriques connexes :** [Guide de référence du langage DMX &#40;Data Mining Extensions&#41;](../../dmx/data-mining-extensions-dmx-reference.md)  
  
 **Générer une nouvelle requête**  
 Créez la requête d'exploration de données en utilisant un outil graphique.  
  
 **Rubriques connexes :** [Data Mining Query](../../integration-services/control-flow/data-mining-query.md)  
  
### <a name="parameter-mapping-tab-options"></a>Options de l'onglet Mappage de paramètre  
 **Nom du paramètre**  
 Vous pouvez également mettre à jour le nom du paramètre. Mappez le paramètre à une variable en sélectionnant cette dernière dans la liste **Nom de la variable** .  
  
 **Nom de la variable**  
 Sélectionnez une variable dans la liste pour l'associer au paramètre.  
  
 **Ajouter**  
 Ajoute un paramètre à la liste.  
  
 **Supprimer**  
 Sélectionnez un paramètre, puis cliquez sur **Supprimer**.  
  
### <a name="result-set-tab-options"></a>Options de l'onglet Ensemble de résultats  
 **Nom de résultat**  
 Vous pouvez également mettre à jour le nom de l'ensemble de résultats. Mappez le résultat à une variable en sélectionnant cette dernière dans la liste **Nom de la variable** .  
  
 Après avoir ajouté un résultat en cliquant sur **Ajouter**, définissez un nom unique pour le résultat.  
  
 **Nom de la variable**  
 Sélectionnez une variable dans la liste pour l'associer à l'ensemble de résultats.  
  
 **Type de résultat**  
 Indique si une ligne ou un ensemble de résultats complet doit être retourné.  
  
 **Ajouter**  
 Ajoute un ensemble de résultats à la liste.  
  
 **Supprimer**  
 Sélectionnez un résultat et cliquez sur **Supprimer**.  
## <a name="data-mining-query-task-editor-output-tab"></a>Éditeur de tâche de requête d'exploration de données (onglet Sortie)
  Utilisez l'onglet **Sortie** de la boîte de dialogue **Éditeur de tâche de requête d'exploration de données** pour définir la destination de la requête de prédiction.  
  
 Pour en savoir plus sur l’implémentation de l’exploration de données dans les packages, consultez [Tâche de requête d’exploration de données](../../integration-services/control-flow/data-mining-query-task.md) et [Solutions d’exploration de données](../../analysis-services/data-mining/data-mining-solutions.md).  
  
### <a name="general-options"></a>Options générales  
 **Nom**  
 Fournissez un nom unique pour la tâche de requête d'exploration de données. Ce nom sert d'étiquette à l'icône de la tâche.  
  
> [!NOTE]  
>  Les noms de tâche doivent être uniques dans un package.  
  
 **Description**  
 Saisissez la description de la tâche de requête d'exploration de données.  
  
### <a name="output-tab-options"></a>Options de l'onglet Sortie  
 **Connexion**  
 Sélectionnez un gestionnaire de connexions dans la liste ou cliquez sur **Nouveau** pour en créer un.  
  
 **Nouveau**  
 Crée un gestionnaire de connexions. Vous ne pouvez utiliser que les types de gestionnaires de connexions ADO.NET et OLE DB.  
  
 **Table de sortie**  
 Définissez la table dans laquelle la requête de prédiction écrit ses résultats.  
  
 **Supprimer et recréer la table de sortie**  
 Indiquez si la requête de prédiction doit remplacer le contenu de la table de destination en supprimant, puis en recréant la table.  
  
