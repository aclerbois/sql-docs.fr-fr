---
title: Gestionnaire de connexions de fichiers plats multiples | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: connection-manager
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.multifile.advanced.f1
- sql13.dts.designer.multifile.columns.f1
- sql13.dts.designer.multifile.general.f1
- sql13.dts.designer.multifile.preview.f1
helpviewer_keywords:
- Multiple Flat Files connection manager
- connections [Integration Services], flat files
- flat files
- flat file connections [Integration Services]
- connection managers [Integration Services], Multiple Flat Files
- multiple flat file connections
ms.assetid: 31fc3f7a-d323-44f5-a907-1fa3de66631a
caps.latest.revision: 41
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: ba43dd74558a2e4506adb516183bc004aee718d3
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="multiple-flat-files-connection-manager"></a>Gestionnaire de connexions de fichiers plats multiples
  Un gestionnaire de connexions de fichiers plats multiples permet à un package d'accéder aux données de plusieurs fichiers plats. Par exemple, une source de fichier plat peut utiliser un gestionnaire de connexions de fichiers plats multiples lorsque la tâche de flux de données se trouve dans un conteneur de boucles (conteneur de boucles For, par exemple). Dans chaque boucle du conteneur, la source de fichier plat charge les données à partir du nom de fichier suivant fourni par le gestionnaire de connexions de fichiers plats multiples.  
  
 Quand vous ajoutez un gestionnaire de connexions de fichiers plats multiples à un package, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] crée un gestionnaire de connexions qui sera résolu en une connexion de fichiers plats multiples au moment de l’exécution, définit les propriétés de la connexion de fichiers plats multiples et ajoute les gestionnaires de connexions de fichiers plats multiples à la collection **Connexions** du package.  
  
 La propriété **ConnectionManagerType** du gestionnaire de connexions a pour valeur **MULTIFLATFILE**.  
  
 Vous pouvez configurer un gestionnaire de connexions de fichiers plats multiples de plusieurs manières :  
  
-   Spécifiez les fichiers, paramètres régionaux et pages de codes à utiliser. Les paramètres régionaux sont utilisés pour interpréter les données spécifiques à un pays comme les dates, tandis que la page de codes est utilisée pour convertir les données chaînes au format Unicode.  
  
-   Spécifiez le format de fichier. Vous pouvez utiliser un format délimité, à largeur fixe ou en drapeau à droite.  
  
-   Spécifiez une ligne d'en-tête, une ligne de données et des séparateurs de colonnes. Les séparateurs de colonnes peuvent être définis au niveau du fichier et remplacés au niveau de la colonne.  
  
-   Indiquez si la première ligne des fichiers contient les noms de colonnes.  
  
-   Spécifiez un caractère d'identificateur de texte. Chaque colonne peut être configurée pour reconnaître un identificateur de texte.  
  
-   Définissez des propriétés comme le nom, le type de données et la largeur maximale pour des colonnes individuelles.  
  
 Lorsque le gestionnaire de connexions de fichiers plats multiples référence plusieurs fichiers, les chemins d'accès aux fichiers sont séparés par une barre verticale (|). La propriété **ConnectionString** du gestionnaire de connexions utilise le format suivant :  
  
 \<*chemin*>|\<*chemin*>  
  
 Vous pouvez également spécifier plusieurs fichiers en utilisant des caractères génériques. Par exemple, pour référencer tous les fichiers texte du lecteur C, vous pouvez affecter la valeur C: ***.txt à la propriété** ConnectionString\\.  
  
 Si un gestionnaire de connexions de fichiers plats multiples référence plusieurs fichiers, tous les fichiers doivent utiliser le même format.  
  
 Par défaut, le gestionnaire de connexions de fichiers plats multiples définit pour les colonnes de type chaîne une longueur de 50 caractères. Dans la boîte de dialogue **Éditeur du gestionnaire de connexions de fichiers plats multiples** , vous pouvez évaluer des exemples de données et redimensionner automatiquement la longueur de ces colonnes pour empêcher la troncation des données ou la largeur de colonne excessive. Sauf si vous redimensionnez la longueur de colonne dans une source de fichier plat ou une transformation, celle-ci reste la même dans le flux de données. Si ces colonnes mappent dans des colonnes de destination plus étroites, des avertissements apparaissent dans l'interface de l'utilisateur et, à l'exécution, des erreurs peuvent se produire du fait de la troncation des données. Vous pouvez redimensionner les colonnes pour les rendre compatibles avec les colonnes de destination dans le gestionnaire de connexions de fichiers plats multiples, la source du fichier plat ou une transformation. Pour modifier la longueur des colonnes de sortie, définissez la propriété **Length** de la colonne de sortie sous l’onglet **Propriétés d’entrée et de sortie** de la boîte de dialogue **Éditeur avancé** .  
  
 Si vous mettez à jour des longueurs de colonne dans le gestionnaire de connexions de fichiers plats multiples après avoir ajouté et configuré la source de fichier plat qui utilise le gestionnaire de connexions, vous n'avez pas à redimensionner manuellement les colonnes de sortie dans la source de fichier plat. Quand vous ouvrez la boîte de dialogue **Source du fichier plat** , la source du fichier plat offre la possibilité de synchroniser les métadonnées de la colonne.  
  
## <a name="configuration-of-the-multiple-flat-files-connection-manager"></a>Configuration du gestionnaire de connexions de fichiers plats multiples  
 Vous pouvez définir les propriétés par le biais du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] ou par programmation.  
  
 Pour plus d’informations sur la configuration d’un gestionnaire de connexions par programmation, consultez <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager> et [Ajout de connexions par programme](../../integration-services/building-packages-programmatically/adding-connections-programmatically.md).  
  
## <a name="multiple-flat-files-connection-manager-editor-general-page"></a>Éditeur du gestionnaire de connexions de fichiers plats multiples (page Général)
  Utilisez la page **Général** de la boîte de dialogue **Éditeur du gestionnaire de connexions de fichiers plats multiples** pour sélectionner un groupe de fichiers ayant le même format de données et pour spécifier leur format de données. Une connexion de fichiers plats multiples permet à un package de se connecter à un groupe de fichiers texte ayant le même format.  
  
 Pour en savoir plus sur le gestionnaire de connexions de fichiers plats multiples, consultez [Multiple Flat Files Connection Manager](../../integration-services/connection-manager/multiple-flat-files-connection-manager.md).  
  
### <a name="options"></a>Options  
 **Nom du gestionnaire de connexions**  
 Spécifiez un nom unique pour la connexion de fichiers plats multiples dans le flux de travail. Le nom fourni sera affiché dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] .  
  
 **Description**  
 Décrit la connexion. Il est recommandé d'indiquer ici l'usage auquel la connexion est destinée, de sorte que les packages soient correctement documentés et plus faciles à gérer.  
  
 **Noms de fichiers**  
 Tapez le chemin d'accès et les noms de fichiers à utiliser dans la connexion de fichiers plats multiples. Vous pouvez spécifier plusieurs fichiers à la fois en utilisant des caractères génériques (par exemple, C:\\*.txt) ou en utilisant la barre verticale (|) pour séparer plusieurs noms de fichiers. Tous les fichiers doivent avoir le même format de données.  
  
 **Parcourir**  
 Naviguez jusqu'aux noms de fichiers à utiliser dans la connexion de fichiers plats multiples. Vous pouvez sélectionner plusieurs fichiers. Tous les fichiers doivent avoir le même format de données.  
  
 **Paramètres régionaux**  
 Indiquez l'emplacement qui fournira les informations de classement et de conversion de la date et de l'heure.  
  
 **Unicode**  
 Indique s'il convient d'utiliser Unicode. L'utilisation d'Unicode vous empêche de spécifier une page de codes.  
  
 **Page de codes**  
 Spécifiez la page de codes pour du texte non-Unicode.  
  
 **Format**  
 Permet de préciser la mise en forme à utiliser : délimitée, à largeur fixe ou en drapeau à droite. Tous les fichiers doivent avoir le même format de données.  
  
|Valeur|Description|  
|-----------|-----------------|  
|Délimité|Les colonnes sont séparées par les séparateurs spécifiés à la page **Colonnes** .|  
|Largeur fixe|Les colonnes ont une largeur fixe que vous spécifiez en faisant glisser les lignes des marqueurs dans la page **Colonnes** .|  
|En drapeau à droite|Dans les fichiers en drapeau à droite, toutes les colonnes ont une largeur fixe, sauf la dernière, qui est délimitée par le séparateur de lignes défini dans la page **Colonnes** .|  
  
 **Qualificateur de texte**  
 Spécifiez le qualificateur de texte à utiliser. Par exemple, vous pouvez spécifier de mettre le texte entre guillemets.  
  
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
 Spécifiez le nombre de lignes d'en-tête à ignorer, le cas échéant.  
  
 **Noms de colonnes dans la première ligne de données**  
 Indique si des noms de colonne doivent être attendus ou fournis dans la première ligne de données.  
  
## <a name="multiple-flat-files-connection-manager-editor-columns-page"></a>Éditeur du gestionnaire de connexions de fichiers plats multiples (page Colonnes)
  Utilisez le nœud **Colonnes** de la boîte de dialogue **Éditeur du gestionnaire de connexions de fichiers plats multiples** pour spécifier les informations de ligne et de colonne et afficher un aperçu du premier fichier sélectionné.  
  
 Pour en savoir plus sur le gestionnaire de connexions de fichiers plats multiples, consultez [Multiple Flat Files Connection Manager](../../integration-services/connection-manager/multiple-flat-files-connection-manager.md).  
  
### <a name="static-options"></a>Options statiques  
 **Nom du gestionnaire de connexions**  
 Spécifiez un nom unique pour la connexion de fichiers plats multiples dans le flux de travail. Le nom fourni sera affiché dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] .  
  
 **Description**  
 Décrit la connexion. Il est recommandé d'indiquer ici l'usage auquel la connexion est destinée, de sorte que les packages soient correctement documentés et plus faciles à gérer.  
  
### <a name="flat-file-format-dynamic-options"></a>Options dynamiques de format de fichier plat  
  
#### <a name="format--delimited"></a>Format = Délimité  
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
  
 **Réinitialiser les colonnes**  
 Cliquez sur **Réinitialiser les colonnes**pour supprimer toutes les colonnes à l’exception de celles d’origine.  
  
#### <a name="format--fixed-width"></a>Format = Largeur fixe  
 **Police**  
 Sélectionnez la police d'affichage de l'aperçu des données.  
  
 **Colonnes de données sources**  
 Ajustez la largeur de la ligne en faisant glisser le marqueur de ligne vertical, et celle des colonnes en cliquant sur la règle en haut de la fenêtre d'aperçu.  
  
 **Largeur de ligne**  
 Spécifiez la largeur de la ligne avant d'ajouter des séparateurs pour des colonnes distinctes. Vous pouvez également faire glisser la ligne verticale dans la fenêtre d'aperçu pour marquer la fin de la ligne. La valeur de la largeur de ligne est automatiquement mise à jour.  
  
 **Réinitialiser les colonnes**  
 Cliquez sur **Réinitialiser les colonnes**pour supprimer toutes les colonnes à l’exception de celles d’origine.  
  
#### <a name="format--ragged-right"></a>Format = En drapeau à droite  
  
> [!NOTE]  
>  Les fichiers en drapeau à droite sont ceux dans lesquels chaque colonne possède une largeur fixe, à l'exception de la dernière. Un séparateur de lignes s'applique.  
  
 **Police**  
 Sélectionnez la police d'affichage de l'aperçu des données.  
  
 **Colonnes de données sources**  
 Ajustez la largeur de la ligne en faisant glisser le marqueur de ligne vertical, et celle des colonnes en cliquant sur la règle en haut de la fenêtre d'aperçu.  
  
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
 Cliquez sur **Réinitialiser les colonnes**pour supprimer toutes les colonnes à l’exception de celles d’origine.  
  
## <a name="multiple-flat-files-connection-manager-editor-advanced-page"></a>Éditeur du gestionnaire de connexions de fichiers plats multiples (page Avancé)
  Utilisez la page **Avancé** de la boîte de dialogue **Éditeur du gestionnaire de connexions de fichiers plats multiples** pour définir des propriétés comme le type de données et les délimiteurs de chaque colonne des fichiers texte auxquels le gestionnaire de connexions de fichiers plats se connecte.  
  
 Par défaut, la longueur des colonnes de type chaîne est de 50 caractères. Vous pouvez évaluer des exemples de données et redimensionner automatiquement la longueur de ces colonnes pour empêcher la troncation des données ou une largeur de colonne excessive. Vous pouvez également mettre à jour d'autres métadonnées pour permettre la compatibilité avec les colonnes de destination. Par exemple, vous pouvez convertir le type de données d'une colonne qui contient uniquement des données de type entier en type de données numérique, tel que DT_I2.  
  
 Pour en savoir plus sur le gestionnaire de connexions de fichiers plats multiples, consultez [Multiple Flat Files Connection Manager](../../integration-services/connection-manager/multiple-flat-files-connection-manager.md).  
  
### <a name="options"></a>Options  
 **Nom du gestionnaire de connexions**  
 Fournissez un nom unique pour le gestionnaire de connexions de fichiers plats multiples dans le flux de travail. Le nom fourni sera affiché dans la zone **Gestionnaires de connexion** du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] .  
  
 **Description**  
 Décrivez le gestionnaire de connexions. Il est recommandé d'indiquer ici l'usage auquel le gestionnaire de connexions est destiné, de sorte que les packages soient correctement documentés et plus faciles à gérer.  
  
 **Configurez les propriétés de chaque colonne**  
 Dans le volet gauche, sélectionnez une colonne pour afficher ses propriétés dans celui de droite. Le tableau ci-dessous fournit une description des définitions des types de données. Certaines de ces propriétés peuvent uniquement être configurées pour certains formats de fichiers plats.  
  
|Propriété|Description|  
|--------------|-----------------|  
|**ColumnType**|Indique si la colonne est délimitée, si elle a une largeur fixe ou si elle présente un format en drapeau à droite. Cette propriété est en lecture seule. Dans les fichiers en drapeau à droite, chaque colonne a une largeur fixe, sauf la dernière qui est arrêtée par le séparateur de lignes.|  
|**OutputColumnWidth**|Indiquez une valeur spécifiant la largeur de colonne en nombre d'octets. Pour les fichiers Unicode, cette valeur est exprimée en nombre de caractères. Dans la tâche de flux de données, cette valeur permet de définir la largeur de la colonne de sortie pour les fichiers plats sources.<br /><br /> Remarque : Dans le modèle objet, le nom de cette propriété est MaximumWidth.|  
|**DataType**|Sélectionnez un type de données dans la liste des types de données disponibles. Pour plus d'informations, consultez [Integration Services Data Types](../../integration-services/data-flow/integration-services-data-types.md).|  
|**TextQualified**|Indiquez si les données texte sont qualifiées à l’aide d’un caractère identificateur de texte :<br /><br /> **True**: les données texte du fichier plat sont qualifiées.<br /><br /> **False**: les données texte du fichier plat ne sont pas qualifiées.|  
|**Nom**|Précisez un nom de colonne. La valeur par défaut est une liste numérotée de colonnes. Vous pouvez toutefois indiquer un nom descriptif unique de votre choix.|  
|**DataScale**|Spécifiez l'échelle des données numériques. L'échelle est le nombre de décimales. Pour plus d'informations, consultez [Integration Services Data Types](../../integration-services/data-flow/integration-services-data-types.md).|  
|**ColumnDelimiter**|Sélectionnez un délimiteur de colonnes dans la liste des séparateurs de colonnes disponibles. Veillez à choisir un caractère de séparation qu'il est peu probable de rencontrer dans le texte. Cette valeur est ignorée dans le cas des colonnes à largeur fixe.<br /><br /> **{CR}{LF}** – Les colonnes sont délimitées par une combinaison retour chariot/saut de ligne<br /><br /> **{CR}** – Les colonnes sont séparées par un retour chariot<br /><br /> **{LF}** – Les colonnes sont séparées par un saut de ligne<br /><br /> **Point-virgule {;}** – Les colonnes sont délimitées par un point-virgule<br /><br /> **Deux-points {:}** – Les colonnes sont délimitées par un deux-points<br /><br /> **Virgule {,}** – Les colonnes sont délimitées par une virgule<br /><br /> **Tabulation {t}** – Les colonnes sont délimitées par une tabulation<br /><br /> **Barre verticale {&#124;}** – Les colonnes sont délimitées par une barre verticale|  
|**DataPrecision**|Spécifiez la précision des données numériques. La précision indique le nombre total de chiffres. Pour plus d'informations, consultez [Integration Services Data Types](../../integration-services/data-flow/integration-services-data-types.md).|  
|**InputColumnWidth**|Indiquez une valeur spécifiant la largeur de colonne en nombre d'octets. Pour les fichiers Unicode, cette valeur est exprimée en nombre de caractères. Cette valeur est ignorée dans le cas des colonnes délimitées.<br /><br /> **Remarque** : Dans le modèle objet, le nom de cette propriété est ColumnWidth.|  
  
 **Nouveau**  
 Ajoutez une nouvelle colonne en cliquant sur **Nouveau**. Par défaut, ce **nouveau** bouton ajoute une nouvelle colonne à la fin de la liste. Il comporte également une liste déroulante avec les options disponibles suivantes :  
  
|Valeur|Description|  
|-----------|-----------------|  
|**Ajouter une colonne**|Ajoute une colonne à la fin de la liste.|  
|**Insérer avant**|Insère une nouvelle colonne avant la colonne sélectionnée.|  
|**Insérer après**|Insère une nouvelle colonne après la colonne sélectionnée.|  
  
 **Supprimer**  
 Sélectionnez une colonne, puis supprimez-la en cliquant sur **Supprimer**.  
  
 **Suggérer les types**  
 Utilisez la boîte de dialogue **Suggérer les types de colonnes** pour évaluer un échantillon de données provenant du premier fichier sélectionné et pour obtenir des suggestions concernant le type de données et la longueur de chaque colonne. Pour plus d’informations, consultez [Référence de l’interface utilisateur de la boîte de dialogue Suggérer les types de colonnes](../../integration-services/connection-manager/suggest-column-types-dialog-box-ui-reference.md).  
  
## <a name="multiple-flat-files-connection-manager-editor-preview-page"></a>Éditeur du gestionnaire de connexions de fichiers plats multiples (page Aperçu)
  La page **Aperçu** de la boîte de dialogue **Éditeur du gestionnaire de connexions de fichiers plats multiples** vous permet d’afficher le contenu du premier fichier source sélectionné, divisé en colonnes, telles que vous les avez définies.  
  
 Pour en savoir plus sur le gestionnaire de connexions de fichiers plats multiples, consultez [Multiple Flat Files Connection Manager](../../integration-services/connection-manager/multiple-flat-files-connection-manager.md).  
  
### <a name="options"></a>Options  
 **Nom du gestionnaire de connexions**  
 Spécifiez un nom unique pour la connexion de fichiers plats multiples dans le flux de travail. Le nom fourni sera affiché dans la zone **Gestionnaires de connexion** du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] .  
  
 **Description**  
 Décrit la connexion. Il est recommandé d'indiquer ici l'usage auquel la connexion est destinée, de sorte que les packages soient correctement documentés et plus faciles à gérer.  
  
 **Lignes de données à ignorer**  
 Indiquez le nombre de lignes qui doivent être ignorées à partir du début du fichier plat.  
  
 **Aperçu des lignes**  
 Permet d'afficher les données d'exemple dans le premier fichier plat sélectionné, divisées en colonnes et en lignes à l'aide des options sélectionnées.  
  
## <a name="see-also"></a> Voir aussi  
 [Source du fichier plat](../../integration-services/data-flow/flat-file-source.md)   
 [Destination de fichier plat](../../integration-services/data-flow/flat-file-destination.md)   
 [Connexions Integration Services &#40;SSIS&#41;](../../integration-services/connection-manager/integration-services-ssis-connections.md)  
  
  
