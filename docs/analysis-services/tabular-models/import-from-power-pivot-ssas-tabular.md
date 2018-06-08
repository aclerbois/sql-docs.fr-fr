---
title: Importer à partir de Power Pivot | Documents Microsoft
ms.date: 05/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tabular-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: 63a1619b09475bfa0ec8d4a1f21aaf88791a5b6c
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="import-from-power-pivot"></a>Importer à partir de Power Pivot 
[!INCLUDE[ssas-appliesto-sqlas-aas](../../includes/ssas-appliesto-sqlas-aas.md)]
  Cet article décrit comment créer un nouveau projet de modèle tabulaire en important les métadonnées et les données d’une [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] classeur à l’aide de l’importation à partir de [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] modèle de projet dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)].  
  
## <a name="create-a-new-tabular-model-from-a-power-pivot-for-excel-file"></a>Créer un nouveau modèle tabulaire à partir d’un fichier Power Pivot pour Excel  
 Lors de la création d’un nouveau projet de modèle tabulaire par importation à partir d’un classeur [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] , les métadonnées qui définissent la structure du classeur sont utilisées pour créer et définir la structure du modèle tabulaire dans [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)]. Les objets tels que les tables, les colonnes, les mesures et les relations sont conservés et apparaîtront dans le projet de modèle tabulaire tels qu’ils s’affichent dans le classeur [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] . Aucune modification n'est apportée au fichier de classeur .xlsx.  
  
> [!NOTE]  
>  Les modèles tabulaires ne prennent pas en charge les tables liées. Pendant l’importation d’un classeur [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] qui contient une table liée, les données de la table liée sont traitées comme des données copiées/collées et stockées dans le fichier Model.bim. Pendant la consultation des propriétés d’une table copiée\collée, la propriété **Données source** est désactivée et la boîte de dialogue **Propriétés de la table** est désactivée dans le menu **Table** .  
>   
>  Il existe une limite de 10 000 lignes qui peuvent être ajoutées aux données incorporées dans le modèle. Si vous importez un modèle depuis [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] et rencontrez l’erreur « Données tronquées. Les tables collées ne peuvent pas contenir plus de 10000 lignes », vous devez modifier le modèle [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] en déplaçant les données incorporées dans une autre source de données, telle qu’une table dans SQL Server, puis réimporter.  
  
 Il existe des considérations spéciales selon que la base de données d'espace de travail se trouve dans une instance Analysis Services sur le même ordinateur (local) que [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] ou dans une instance Analysis Services distante.  
  
 Si la base de données d’espace de travail se trouve sur une instance Analysis Services locale, vous pouvez importer les métadonnées et les données depuis le classeur [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] . Les métadonnées sont copiées depuis le classeur et utilisées pour créer le projet de modèle tabulaire. Les données sont ensuite copiées depuis le classeur et stockées dans la base de données d'espace de travail du projet (sauf pour les données copiées/collées, qui sont stockées dans le fichier Model.bim).  
  
 Si la base de données d’espace de travail se trouve dans une instance Analysis Services distante, vous ne pouvez pas importer de données depuis un classeur [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour Excel. Vous pouvez toujours importer les métadonnées de classeur ; toutefois, cela provoque l'exécution d'un script sur l'instance Analysis Services distante. Vous devez uniquement importer des métadonnées d’un classeur [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] approuvé. Les données doivent être importées à partir des sources définies dans les connexions à la source de données. Les données de table liée et copiées/collées dans le classeur [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] doivent être copiées et collées dans le projet de modèle tabulaire.  
  
#### <a name="to-create-a-new-tabular-model-project-from-a-power-pivot-for-excel-file"></a>Pour créer un nouveau projet de modèle tabulaire à partir d’un fichier Power Pivot pour Excel  
  
1.  Dans le menu [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)]Fichier **de** , cliquez sur **Nouveau**, puis sur **Projet**.  
  
2.  Dans la boîte de dialogue **Nouveau projet**, sous **Modèles installés**, cliquez sur **Business Intelligence**, puis sur **Importer depuis [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)]**.  
  
3.  Pour le  **Nom**, tapez un nom pour le projet, puis spécifiez un emplacement et le nom de la solution et cliquez sur **OK**.  
  
4.  Dans la boîte de dialogue **Ouvrir** , sélectionnez le fichier [!INCLUDE[ssGeminiClient](../../includes/ssgeminiclient-md.md)] qui contient les données et les métadonnées de modèle que vous souhaitez importer, puis cliquez sur **Ouvrir**.  
  
## <a name="see-also"></a>Voir aussi  
 [Base de données de l’espace de travail](../../analysis-services/tabular-models/workspace-database-ssas-tabular.md)   
 [Copier et coller des données](../../analysis-services/tabular-models/ssas-import-data-copy-and-paste-data.md)  
  
  
