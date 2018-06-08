---
title: Traductions dans les modèles tabulaires (Analysis Services) | Documents Microsoft
ms.date: 05/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tabular-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: ddd49ce6d3edc3f1e2f72a3fe7f5ab61621eef62
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="translations-in-tabular-models-analysis-services"></a>Traductions dans les modèles tabulaires (Analysis Services)
[!INCLUDE[ssas-appliesto-sqlas-aas](../../includes/ssas-appliesto-sqlas-aas.md)]
  [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] Ajoute la prise en charge des chaînes de traduction pour les modèles tabulaires. Un objet unique dans le modèle peut avoir plusieurs traductions de nom ou de description, ce qui permet de prendre en charge des versions multilingues dans la définition du modèle.  
  
 Les chaînes traduites sont destinées aux métadonnées d’objet uniquement (noms et descriptions des tables et colonnes) qui s’affichent dans un outil client tel qu’une liste de tableau croisé dynamique Excel.  Pour utiliser des chaînes traduites, la connexion cliente spécifie la culture. Dans la fonctionnalité **Analyse dans Excel** , vous pouvez choisir la langue dans la liste déroulante. Pour d’autres outils, vous devrez peut-être spécifier la culture dans la chaîne de connexion.  
  
 Cette fonctionnalité n’est pas prévue pour charger les données traduites dans un modèle. Si vous souhaitez charger les valeurs des données traduites, vous devez développer une stratégie de traitement qui inclut l’extraction des chaînes traduites à partir de la source de données qui les fournit.  
  
 Voici un flux de travail typique pour l’ajout de métadonnées traduites :  
  
-   Générer un fichier JSON de traduction vide qui contient des espaces réservés pour chaque traduction de chaîne  
  
-   Ajouter des traductions de chaîne dans le fichier JSON  
  
-   Réimporter les traductions dans le modèle  
  
-   Générer, traiter ou déployer le modèle  
  
-   Se connecter au modèle à l’aide d’une application cliente qui accepte un LCID dans la chaîne de connexion  
  
## <a name="create-an-empty-translation-file"></a>Créer un fichier de traduction vide  
 [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] permet d’ajouter des traductions.  
  
1.  Cliquez sur **Modèles** > **Traductions** > **Gérer les traductions**.  
  
2.  Sélectionnez les langues pour lesquelles vous fournissez des traductions, puis cliquez sur **Ajouter**.  
  
3.  Choisissez une ou plusieurs langues dans la liste, en fonction de la façon dont vous souhaitez importer les chaînes ultérieurement.  
  
     Votre fichier de traduction peut inclure plusieurs langues, mais la gestion des traductions est plus facile si vous créez un fichier de traduction par langue. Le fichier de traduction que vous créez maintenant sera importé dans son intégralité ultérieurement. Pour modifier les options d’importation par langue, chaque langue doit avoir son propre fichier.  
  
4.  Cliquez sur **Exporter le fichier de langue**.  Indiquez un nom de fichier et un emplacement.  
  
 ![ssas-tabular-translate-export](../../analysis-services/tabular-models/media/ssas-tabular-translate-export.png "ssas-tabular-translate-export")  
  
## <a name="add-translations"></a>Ajouter des traductions  
 Un fichier de traduction JSON vide inclut des métadonnées pour les traductions propres à une langue. Les espaces réservés aux traductions pour les noms d’objets et les descriptions sont spécifiés dans la section **Culture** à la fin de la définition du modèle. Des traductions peuvent être ajoutées pour les éléments suivants :  
  
|||  
|-|-|  
|translatedCaption|Titre de table ou de colonne qui s’affiche dans les applications clientes prenant en charge les visualisations d’un modèle tabulaire.|  
|translatedDescription|Moins fréquentes que les légendes. Une description s’affiche en tant qu’informations de modèle dans un outil de modélisation comme SSDT.|  
  
 Ne supprimez pas les métadonnées non spécifiées du fichier.  Elles doivent correspondre au fichier sur lequel elles sont basées. Ajoutez simplement les chaînes de votre choix, puis enregistrez le fichier.  
  
 Bien que cela puisse être considéré comme une erreur, la section  **referenceCulture** contient les métadonnées dans la culture par défaut du modèle. Les modifications apportées à la section **referenceCulture** ne sont pas lues pendant l’importation et sont ignorées.  
  
 L’exemple suivant montre des légendes et une description traduites pour les tables **DimProduct** et **DimCustomer** .  
  
 ![ssas-tabular-translate-json](../../analysis-services/tabular-models/media/ssas-tabular-translate-json.png "ssas-tabular-translate-json")  
  
> [!TIP]  
>  Vous pouvez utiliser n’importe quel éditeur JSON pour ouvrir le fichier, mais nous vous recommandons d’utiliser l'éditeur JSON de Visual Studio. Vous pouvez ainsi également utiliser la commande Afficher le code dans l’Explorateur de solutions pour afficher la définition de modèle tabulaire dans SSDT. Pour obtenir l’éditeur JSON, vous avez besoin d’une [installation complète de Visual Studio 2015](https://www.visualstudio.com/en-us/downloads/download-visual-studio-vs.aspx). L’édition Community gratuite inclut l’éditeur JSON.  
  
## <a name="import-a-translation-file"></a>Importer un fichier de traduction  
 Les chaînes de traduction que vous importez deviennent partie intégrante de la définition du modèle. Une fois que les chaînes sont importées, le fichier de traduction n’est plus référencé.  
  
1.  Cliquez sur **Modèles** > **Traductions** > **Importer des traductions**.  
  
2.  Recherchez le fichier de traduction, puis cliquez sur **Ouvrir**.  
  
3.  Vous pouvez également spécifier des options d’importation.  
  
    |||  
    |-|-|  
    |Remplacer les traductions existantes|Remplace toutes les légendes ou descriptions existantes de la même langue que le fichier importé.|  
    |Ignorer les objets non valides|Spécifie si les différences de métadonnées doivent être ignorées ou marquées comme erreur.|  
    |Écrire les résultats d’importation dans un fichier journal|Par défaut, les fichiers journaux sont enregistrés dans le dossier du projet. Le chemin d’accès exact du fichier est fourni une fois l’importation terminée. Nom du fichier journal est SSDT_Translations_Log_\<horodatage >.|  
    |Sauvegarder les traductions dans un fichier JSON avant l’importation|Sauvegarde une traduction existante qui correspond à la culture des chaînes importées.  Si la culture en cours d’importation n’est pas présente dans le modèle, la sauvegarde est vide.<br /><br /> Si vous devez restaurer ce fichier ultérieurement, vous pouvez remplacer le contenu de model.bim par ce fichier JSON.|  
  
4.  Cliquez sur **Importer**.  
  
5.  Éventuellement, si vous avez généré un fichier journal ou une sauvegarde, vous pouvez trouver les fichiers dans le dossier du projet (par exemple, C:\Users\Documents\Visual Studio 2015\Projects\Tabular1200-AW\Tabular1200-AW).  
  
6.  Pour vérifier l’importation, procédez comme suit :  
  
    -   Cliquez avec le bouton droit sur le fichier **model.bim** dans l’Explorateur de solutions et choisissez **Afficher le code**. Cliquez sur **Oui** pour fermer le mode de création et rouvrir **model.bim** en mode Code.  Si vous avez installé une version complète de Visual Studio, notamment l’édition Community gratuite, le fichier s’ouvre dans l’éditeur JSON intégré.  
  
    -   Recherchez **Culture** ou une chaîne traduite spécifique pour vérifier que les chaînes apparaissent bien comme vous le souhaitez.  
  
## <a name="connect-using-a-locale-identifier"></a>Se connecter à l’aide d’un identificateur de paramètres régionaux  
 Cette section décrit une approche qui permet de valider les chaînes renvoyées à partir du modèle.  
  
1.  Dans Excel, connectez-vous au modèle tabulaire. Cette étape suppose que le modèle a été déployé. Si le modèle n’existe que dans l’espace de travail, déployez-le sur une instance Analysis Services pour effectuer le contrôle de validation.  
  
     Vous pouvez également utiliser la fonctionnalité **Analyser dans Excel** pour vous connecter au modèle.  
  
2.  Dans la boîte de dialogue Connexion Excel, choisissez la culture pour laquelle des traductions de chaîne existent dans votre modèle. Excel détecte les cultures définies dans le modèle et remplit la liste déroulante en conséquence.  
  
     ![ssas-tabular-translations-excel](../../analysis-services/tabular-models/media/ssas-tabular-translations-excel.png "ssas-tabular-translations-excel")  
  
     Lorsque vous créez un tableau croisé dynamique, vous devez voir le tableau et les noms de colonnes traduits.  
  
## <a name="see-also"></a>Voir aussi  
 [Niveau de compatibilité pour les modèles tabulaires dans Analysis Services](../../analysis-services/tabular-models/compatibility-level-for-tabular-models-in-analysis-services.md)   
 [Scénarios de globalisation pour Analysis Services](../../analysis-services/globalization-scenarios-for-analysis-services.md)   
 [Analyser dans Excel](../../analysis-services/tabular-models/analyze-in-excel-ssas-tabular.md)  
  
  
