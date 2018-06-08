---
title: Se connecter à une source de données de fichier plat (Assistant Importation et Exportation SQL Server) | Microsoft Docs
ms.custom: ''
ms.date: 02/17/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: import-export-data
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: d7e7067b-f5a5-482f-b97e-9d82fe8e9f76
caps.latest.revision: 8
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 73e11ad9c4da2f1a87eb7c148b9a37688273c8ce
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="connect-to-a-flat-file-data-source-sql-server-import-and-export-wizard"></a>Se connecter à une source de données de fichier plat (Assistant Importation et Exportation SQL Server)
Cette rubrique vous montre comment se connecter à une source de données de **fichier plat** (fichier texte) à partir de la page **Choisir une source de données** ou **Choisir une destination** de l’Assistant Importation et Exportation SQL Server. Ces deux pages de l’Assistant présentent des jeux d’options différents pour les fichiers plats, ainsi cette rubrique décrit la source de fichier plat et la destination de fichier plat séparément.

## <a name="an-alternative-for-simple-text-import"></a>Une alternative pour l’importation de texte simple
Si vous devez importer un fichier texte dans SQL Server et que vous n’avez pas besoin de tous les options de configuration qui sont disponibles dans l’Assistant Importation et Exportation, envisagez d’utiliser l**’Assistant Importation de fichier plat** dans SQL Server Management Studio (SSMS). Pour plus d’informations, consultez les articles suivants :
- [Nouveautés de SQL Server Management Studio 17.3 ](https://blogs.technet.microsoft.com/dataplatforminsider/2017/10/10/whats-new-in-sql-server-management-studio-17-3/)
- [Présentation du nouvel Assistant Importation de fichier plat dans SSMS 17.3](https://channel9.msdn.com/Shows/Data-Exposed/Introducing-the-new-Import-Flat-File-Wizard-in-SSMS-173)

## <a name="connect-to-a-flat-file-source"></a>Se connecter à une source de fichier plat
 
 L’utilisation de fichiers plats comme sources de données représente quatre pages d’options. Cela fait un grand nombre de pages ! Toutefois, vous n’êtes pas obligé de passer trop de temps sur chaque page. Voici les tâches à prendre en compte.
 
Radiomessagerie|Recommandation  |Type  
----|---------|---------
**Général**|Assurez-vous de mettre à jour les options de la section **Format**.|Recommandation    
**Colonnes**|Veillez à contrôler les délimiteurs de colonnes et de lignes (pour un fichier délimité), ou de marquer les colonnes (pour un fichier de largeur fixe).|Recommandation
**Avancé**|Si vous le souhaitez, vérifiez les types de données ainsi que d’autres propriétés attribuées par défaut aux colonnes.|Ce paramètre est facultatif
**Aperçu**|Le cas échéant, prévisualisez un échantillon de données en utilisant les paramètres que vous avez spécifiés.|Ce paramètre est facultatif

## <a name="general-page-source"></a>Page Général (source)
 Dans la page **Général**, accédez au fichier, puis vérifiez les paramètres de la section **Format**.
 
 ![Page Général de la connexion à un fichier plat](../../integration-services/import-export-data/media/flat-file-connection-general.png)  

### <a name="options-to-specify-general-page"></a>Options à spécifier (Page **Général**)

 **Nom de fichier**  
 Entrez le chemin et le nom du fichier plat.  
  
 **Parcourir**  
 Recherchez le fichier plat.  
  
 **Paramètres régionaux**  
 Spécifiez les paramètres régionaux afin de fournir les informations spécifiques à la langue pour le tri et les formats de date et d’heure.  
  
 **Unicode**  
 Spécifiez si le fichier utilise Unicode. Si vous utilisez Unicode, vous ne pouvez pas spécifier de page de codes.  
  
 **Page de codes**  
 Spécifiez la page de codes pour du texte non-Unicode.  
  
 **Format**  
 Précisez si le fichier utilise un format délimité, à largeur fixe ou en drapeau à droite.  
  
|Valeur|Description|  
|-----------|-----------------|  
|Délimité|Les colonnes sont séparées par des délimiteurs. Vous spécifiez le délimiteur dans la page **Colonnes**.|  
|Largeur fixe|Les colonnes ont une largeur fixe.|  
|En drapeau à droite|Dans les fichiers en drapeau à droite, chaque colonne a une largeur fixe, sauf la dernière qui est délimitée par le séparateur de lignes.|  
  
 **Qualificateur de texte**  
 Le cas échéant, spécifiez l’identificateur de texte utilisé par le fichier. Par exemple, vous pouvez spécifier que les champs de texte soient placés entre guillemets. (Cette propriété s’applique uniquement aux fichiers délimités.) 
  
> [!NOTE]
> Après avoir sélectionné un identificateur de texte, vous ne pouvez pas sélectionner de nouveau l’option **Aucun**. Tapez **Aucun** pour désélectionner l’identificateur de texte.  
  
 **Séparateur de lignes d'en-tête**  
 Choisissez dans la liste des séparateurs de lignes d'en-tête ou entrez le texte de séparation.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**{CR}{LF}**|La ligne d'en-tête est séparée par une combinaison retour chariot-saut de ligne.|  
|**{CR}**|La ligne d'en-tête est séparée par des retours chariot.|  
|**{LF}**|La ligne d'en-tête est séparée par des sauts de lignes.|  
|**Point-virgule {;}**|La ligne d'en-tête est séparée par des points-virgules.|  
|**Deux-points {:}**|La ligne d'en-tête est séparée par des deux-points.|  
|**Virgule {,}**|La ligne d'en-tête est séparée par des virgules.|  
|**Tabulation {t}**|La ligne d'en-tête est séparée par des tabulations.|  
|**Barre verticale {&#124;}**|La ligne d'en-tête est séparée par des barres verticales.|  
  
 **Lignes d'en-tête à ignorer**  
 Spécifiez le nombre de lignes à ignorer en haut du fichier, le cas échéant.  
  
 **Noms de colonnes dans la première ligne de données**  
 Spécifiez si la première ligne (après les lignes ignorées) contient des noms de colonne.

## <a name="columns-page---format--delimited-source"></a>Page Colonnes - Format = Délimité (source)
 Dans la page **Colonnes**, vérifiez la liste des colonnes et les délimiteurs identifiés par l’Assistant. La capture d’écran suivante montre la page lorsque vous avez sélectionné **Délimité** comme format de fichier plat.
 
![Page Colonnes, délimité, fichier plat](../../integration-services/import-export-data/media/flat-file-delimited-columns-page.jpg)

### <a name="options-to-specify-columns-page---format--delimited"></a>Options à spécifier (page **Colonnes** - Format = Délimité)

 **Séparateur de lignes**  
 Effectuez une sélection dans la liste des séparateurs de lignes disponibles ou entrez le texte du séparateur.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**{CR}{LF}**|Les lignes sont séparées par une combinaison de retour chariot/saut de ligne.|  
|**{CR}**|Les lignes sont séparées par un retour chariot.|  
|**{LF}**|Les lignes sont séparées par un saut de ligne.|  
|**Point-virgule {;}**|Les lignes sont séparées par un point-virgule.|  
|**Deux-points {:}**|Les lignes sont séparées par un deux-points.|  
|**Virgule {,}**|Les lignes sont séparées par une virgule.|  
|**Tabulation {t}**|Les lignes sont séparées par une tabulation.|  
|**Barre verticale {&#124;}**|Les lignes sont séparées par une barre verticale.|  
  
 **Délimiteur de colonne**  
 Effectuez une sélection dans la liste des séparateurs de colonnes disponibles ou entrez le texte du séparateur.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**{CR}{LF}**|Les colonnes sont délimitées par une combinaison retour chariot-saut de ligne.|  
|**{CR}**|Les colonnes sont séparées par un retour chariot.|  
|**{LF}**|Les colonnes sont séparées par un saut de ligne.|  
|**Point-virgule {;}**|Les colonnes sont séparées par un point-virgule.|  
|**Deux-points {:}**|Les colonnes sont séparées par un deux-points.|  
|**Virgule {,}**|Les colonnes sont séparées par une virgule.|  
|**Tabulation {t}**|Les colonnes sont séparées par une tabulation.|  
|**Barre verticale {&#124;}**|Les colonnes sont séparées par une barre verticale.|  
  
 **Aperçu des lignes**  
 Affichez les données d'exemple dans le fichier plat, divisées en colonnes et en lignes à l'aide des options sélectionnées.  
 
 **Actualiser**  
 Affichez les résultats des modifications des séparateurs en cliquant sur **Actualiser**. Il ne devient visible qu'après avoir changé d'autres options de connexion.  
  
 **Réinitialiser les colonnes**  
 Restaurer les colonnes d’origine.  

## <a name="columns-page---format--fixed-width-source"></a>Page Colonnes - Format = Largeur fixe (source)
Dans la page **Colonnes**, vérifiez la liste des colonnes et les délimiteurs identifiés par l’Assistant. La capture d’écran suivante montre la page lorsque vous avez sélectionné **Largeur fixe** comme format de fichier plat.
  
![Page Colonnes, largeur fixe, fichier plat](../../integration-services/import-export-data/media/flat-file-fixed-width-columns-page.jpg)

### <a name="options-to-specify-columns-page---format--fixed-width"></a>Options à spécifier (page **Colonnes** - Format = Largeur fixe)

 **Police**  
 Sélectionnez la police d'affichage de l'aperçu des données.  
  
 **Colonnes de données sources**  
 Ajustez la largeur de la ligne en faisant glisser le marqueur de ligne rouge vertical, et celle des colonnes en cliquant sur la règle en haut de la fenêtre d'aperçu.  
  
 **Largeur de ligne**  
 Spécifiez la largeur de la ligne avant d'ajouter des séparateurs pour des colonnes distinctes. Vous pouvez également faire glisser la ligne rouge verticale dans la fenêtre d'aperçu pour marquer la fin de la ligne. La valeur de la largeur de ligne est automatiquement mise à jour.  
  
 **Réinitialiser les colonnes**  
 Restaurer les colonnes d’origine.  
  
## <a name="columns-page---format--ragged-right-source"></a>Page Colonnes - Format = En drapeau à droite (source)
Dans la page **Colonnes**, vérifiez la liste des colonnes et les délimiteurs identifiés par l’Assistant. La capture d’écran suivante montre la page lorsque vous avez sélectionné **En drapeau à droite** comme format de fichier plat.

> [!NOTE]
> Dans les fichiers en drapeau à droite, chaque colonne a une largeur fixe, sauf la dernière qui est délimitée par le séparateur de lignes.  
 
![Fichier plat, en drapeau à droite, page Colonnes](../../integration-services/import-export-data/media/flat-file-ragged-right-columns-page.jpg)

### <a name="options-to-specify-columns-page---format--ragged-right"></a>Options à spécifier (page **Colonnes** - Format = En drapeau à droite)
   
 **Police**  
 Sélectionnez la police d'affichage de l'aperçu des données.  
  
 **Colonnes de données sources**  
 Ajustez la largeur de la ligne en faisant glisser le marqueur de ligne rouge vertical, et celle des colonnes en cliquant sur la règle en haut de la fenêtre d'aperçu.  
  
 **Séparateur de lignes**  
 Effectuez une sélection dans la liste des séparateurs de lignes disponibles ou entrez le texte du séparateur.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**{CR}{LF}**|Les lignes sont séparées par une combinaison de retour chariot/saut de ligne.|  
|**{CR}**|Les lignes sont séparées par un retour chariot.|  
|**{LF}**|Les lignes sont séparées par un saut de ligne.|  
|**Point-virgule {;}**|Les lignes sont séparées par un point-virgule.|  
|**Deux-points {:}**|Les lignes sont séparées par un deux-points.|  
|**Virgule {,}**|Les lignes sont séparées par une virgule.|  
|**Tabulation {t}**|Les lignes sont séparées par une tabulation.|  
|**Barre verticale {&#124;}**|Les lignes sont séparées par une barre verticale.|  
  
 **Réinitialiser les colonnes**  
 Restaurer les colonnes d’origine.  

## <a name="advanced-page-source"></a>Page Avancé (source)
La page **Avancé** affiche des informations détaillées sur chaque colonne de la source de données, notamment son type de données et sa taille. La capture d’écran suivante montre la page **Avancé** pour la première colonne dans un fichier plat délimité.

![Page Avancé, délimité, fichier plat](../../integration-services/import-export-data/media/flat-file-delimited-advanced-page.jpg)

Dans la capture d’écran, remarquez que les données de la colonne **id**, qui contient des chiffres, sont initialement de type chaîne.

### <a name="options-to-specify-advanced-page"></a>Options à spécifier (page **Avancé**)

 **Configurez les propriétés de chaque colonne**  
 Dans le volet gauche, sélectionnez une colonne pour afficher ses propriétés dans celui de droite. Le tableau suivant fournit une description des propriétés de colonne. Certaines de ces propriétés sont configurables uniquement pour quelques formats de fichier plat et des colonnes de certains types de données.  
  
|Propriété|Description|  
|--------------|-----------------|  
|**Nom**|Précisez un nom de colonne descriptif. Si vous n’entrez aucun nom, [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] crée automatiquement un nom au format Colonne 0, Colonne 1, et ainsi de suite.|
|**ColumnDelimiter**|Sélectionnez un délimiteur de colonnes dans la liste des séparateurs de colonnes disponibles. Veillez à choisir un caractère de séparation qu'il est peu probable de rencontrer dans le texte. Cette valeur est ignorée dans le cas des colonnes à largeur fixe.<br /><br /> **{CR}{LF}**. Les colonnes sont délimitées par une combinaison retour chariot-saut de ligne.<br /><br /> **{CR}**. Les colonnes sont séparées par un retour chariot.<br /><br /> **{LF}**. Les colonnes sont séparées par un saut de ligne.<br /><br /> **Point-virgule {;}**. Les colonnes sont séparées par un point-virgule.<br /><br /> **Deux-points {:}**. Les colonnes sont séparées par un deux-points.<br /><br /> **Virgule {,}**. Les colonnes sont séparées par une virgule.<br /><br /> **Tabulation {t}**. Les colonnes sont séparées par une tabulation.<br /><br /> **Barre verticale {&#124;}**. Les colonnes sont séparées par une barre verticale.|
|**ColumnType**|Indique si la colonne est délimitée, si elle a une largeur fixe ou si elle présente un format en drapeau à droite. Cette propriété est en lecture seule. Les fichiers en drapeau à droite sont des fichiers dans lesquels chaque colonne a une largeur fixe, à l'exception de la dernière colonne. Un séparateur de lignes s'applique.|  
|**InputColumnWidth**|Indiquez une valeur spécifiant la largeur de colonne à conserver en nombre d’octets. Pour les fichiers Unicode, cette valeur est un nombre de caractères. Cette valeur est ignorée dans le cas des colonnes délimitées.<br /><br /> **Remarque** : Dans le modèle objet, le nom de cette propriété est ColumnWidth.|
|**DataPrecision**|Spécifiez la précision des données numériques. La précision indique le nombre total de chiffres.|
|**DataScale**|Spécifiez l'échelle des données numériques. L'échelle est le nombre de décimales.|
|**DataType**|Sélectionnez un type de données dans la liste des types de données disponibles.<br/>Pour plus d'informations, consultez [Integration Services Data Types](../../integration-services/data-flow/integration-services-data-types.md).|
|**OutputColumnWidth**|Indiquez une valeur spécifiant la largeur de colonne en nombre d'octets. Pour les fichiers Unicode, cette valeur correspond à un nombre de caractères. Dans la tâche de flux de données, cette valeur permet de définir la largeur de la colonne de sortie pour les fichiers plats sources. Dans le modèle objet, le nom de la propriété est MaximumWidth.|  
|**TextQualified**|Indique si les données de texte sont entourées par des caractères identificateurs de texte, tels que des caractères de guillemets.<br /><br /> True : les données texte du fichier plat sont qualifiées. False : les données texte du fichier plat ne sont pas qualifiées.|  
  
**Nouveau**  
 Ajoutez une nouvelle colonne en cliquant sur **Nouveau**. Par défaut, ce **nouveau** bouton ajoute une nouvelle colonne à la fin de la liste. Le bouton possède également les options ci-dessous, disponibles dans la liste déroulante.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**Ajouter une colonne**|Ajoute une colonne à la fin de la liste.|  
|**Insérer avant**|Insère une nouvelle colonne avant la colonne sélectionnée.|  
|**Insérer après**|Insère une nouvelle colonne après la colonne sélectionnée.|  
  
 **Supprimer**  
 Sélectionnez une colonne, puis supprimez-la en cliquant sur **Supprimer**.  
  
 **Suggérer les types**  
 La boîte de dialogue **Suggérer les types de colonnes** permet d’évaluer des échantillons de données dans le fichier et d’obtenir des suggestions pour le type de données et la longueur de chaque colonne.  
 
Cliquez sur **Suggérer les types** pour afficher la boîte de dialogue **Suggérer les types de colonnes**. 

![Boîte de dialogue Suggérer les types pour une connexion à un fichier plat](../../integration-services/import-export-data/media/flat-file-connection-suggest.png)

Une fois que vous avez choisi les options dans la boîte de dialogue **Suggérer les types de colonnes** et que vous avez cliqué sur **OK**, l’Assistant peut modifier les types de données de certaines colonnes.

Dans la capture d’écran suivante, après que vous avez cliqué sur **Suggérer les types**, l’Assistant a reconnu que la colonne **id** de la source de données est en fait un nombre et non une chaîne de texte ; il a donc remplacé le type chaîne des données de la colonne par le type entier.

![Page Avancé de la connexion à un fichier plat (après)](../../integration-services/import-export-data/media/flat-file-connection-advanced-after.png)

Pour plus d’informations, consultez [Référence de l’interface utilisateur de la boîte de dialogue Suggérer les types de colonnes](../../integration-services/connection-manager/suggest-column-types-dialog-box-ui-reference.md).

## <a name="preview-page-source"></a>Page Aperçu (source)

Dans la page **Aperçu**, vérifiez que la liste des colonnes et que les exemples de données sont conformes à vos attentes.

![Page Aperçu, fichier plat](../../integration-services/import-export-data/media/flat-file-preview-page.jpg)

### <a name="options-to-specify-preview-page"></a>Options à spécifier (page **Aperçu**)

 **Lignes de données à ignorer**  
 Indiquez le nombre de lignes qui doivent être ignorées à partir du début du fichier plat.  
  
 **Aperçu des lignes**  
 Affiche un échantillon de données du fichier plat, divisé en colonnes et en lignes en fonction des options que vous avez sélectionnées.
 
 **Actualiser**  
 Cliquez sur le bouton **Actualiser**pour visualiser l’effet obtenu en modifiant le nombre de lignes à ignorer. Il ne devient visible qu'après avoir changé d'autres options de connexion.  
 
Pour plus d’informations sur la page **Aperçu**, consultez la page [Éditeur du gestionnaire de connexions de fichiers plats &#40;page Aperçu&#41;](../../integration-services/connection-manager/flat-file-connection-manager-editor-preview-page.md) dans les informations de référence sur Integration Services.

## <a name="connect-to-a-flat-file-destination"></a>Se connecter à une destination de fichier plat
Pour une destination de fichier plat, il n’existe qu’une seule page d’options, comme illustré dans la capture d’écran suivante. Accédez au fichier et sélectionnez-le, vérifiez ensuite les paramètres de la section **Format**.

![Connexion à une destination de fichier plat](../../integration-services/import-export-data/media/connect-to-flat-file-destination.jpg)

### <a name="options-to-specify-choose-a-destination-page"></a>Options à spécifier (page **Choisir une Destination**)

 **Nom de fichier**  
 Entrez le chemin et le nom du fichier plat.  
  
 **Parcourir**  
 Recherchez le fichier plat.  
  
 **Paramètres régionaux**  
 Spécifiez les paramètres régionaux afin de fournir les informations spécifiques à la langue pour le tri et les formats de date et d’heure.  
  
 **Unicode**  
 Spécifiez si le fichier utilise Unicode. Si vous utilisez Unicode, vous ne pouvez pas spécifier de page de codes.  
  
 **Page de codes**  
 Spécifiez la page de codes pour du texte non-Unicode.  
  
 **Format**  
 Précisez si le fichier utilise un format délimité, à largeur fixe ou en drapeau à droite.  
  
|Valeur|Description|  
|-----------|-----------------|  
|Délimité|Les colonnes sont séparées par des délimiteurs. Vous spécifiez le délimiteur dans la page **Colonnes**.|  
|Largeur fixe|Les colonnes ont une largeur fixe.|  
|En drapeau à droite|Dans les fichiers en drapeau à droite, chaque colonne a une largeur fixe, sauf la dernière qui est délimitée par le séparateur de lignes.|  
  
 **Qualificateur de texte**  
 Le cas échéant, spécifiez l’identificateur de texte utilisé par le fichier. Par exemple, vous pouvez spécifier que les champs de texte soient placés entre guillemets. (Cette propriété s’applique uniquement aux fichiers délimités.) 
  
> [!NOTE] 
> Après avoir sélectionné un identificateur de texte, vous ne pouvez pas sélectionner de nouveau l’option **Aucun**. Tapez **Aucun** pour désélectionner l’identificateur de texte.  

## <a name="see-also"></a>Voir aussi
[Choisir une source de données](../../integration-services/import-export-data/choose-a-data-source-sql-server-import-and-export-wizard.md)  
[Choisir une destination](../../integration-services/import-export-data/choose-a-destination-sql-server-import-and-export-wizard.md)

