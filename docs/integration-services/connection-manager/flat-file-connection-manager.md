---
title: Gestionnaire de connexions de fichiers plats | Microsoft Docs
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
- sql13.dts.designer.ffileconnection.general.f1
- sql13.dts.designer.ffileconnection.columns.f1
- sql13.dts.designer.ffileconnection.columnproperties.f1
- sql13.dts.designer.ffileconnection.preview.f1
helpviewer_keywords:
- connection managers [Integration Services], Flat File
- connections [Integration Services], flat files
- files [Integration Services], connections
- Flat File connection manager
- flat files
- flat file connections [Integration Services]
ms.assetid: 7830f80d-af32-4e8f-a6fc-f03af6bc1946
caps.latest.revision: 49
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: fe024751fe5e9510cb7fd4a69e7bbd120a1539b5
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="flat-file-connection-manager"></a>Gestionnaire de connexions de fichiers plats
  Un gestionnaire de connexions de fichiers plats permet à un package d'accéder aux données d'un fichier plat. Ainsi, les sources et destinations de fichiers plats peuvent utiliser des gestionnaires de connexions de fichiers plats pour extraire et charger des données.  
  
 Le gestionnaire de connexions de fichiers plats peut accéder à un seul fichier. Pour référencer plusieurs fichiers, utilisez un gestionnaire de connexions de fichiers plats multiples plutôt qu'un gestionnaire de connexions de fichiers plats. Pour plus d’informations, consultez [Gestionnaire de connexion de fichiers plats multiples](../../integration-services/connection-manager/multiple-flat-files-connection-manager.md).  
  
## <a name="column-length"></a>Longueur de colonne  
 Par défaut, le gestionnaire de connexions de fichiers plats définit la longueur des colonnes de chaînes à 50 caractères. Dans la boîte de dialogue **Éditeur du gestionnaire de connexions de fichiers plats** , vous pouvez évaluer les exemples de données et redimensionner automatiquement la longueur de ces colonnes pour empêcher la troncation de données ou une largeur de colonnes excessive. En outre, sauf si vous redimensionnez ultérieurement la longueur de colonne dans une source de fichiers plats ou une transformation, la longueur de colonne de la colonne de chaîne reste la même dans tout le flux de données. Si ces colonnes de chaînes sont mappées à des colonnes de destination plus étroites, des avertissements apparaissent dans l'interface utilisateur. En outre, au moment de l'exécution, des erreurs peuvent se produire en raison de la troncation des données. Pour éviter les erreurs ou la troncation, vous pouvez redimensionner les colonnes pour assurer leur compatibilité avec les colonnes de destination dans le gestionnaire de connexions de fichiers plats, la source de fichiers plats ou une transformation. Pour modifier la longueur des colonnes de sortie, définissez la propriété **Length** de la colonne de sortie sous l’onglet **Propriétés d’entrée et de sortie** de la boîte de dialogue **Éditeur avancé** .  
  
 Si vous mettez à jour les longueurs de colonnes dans le gestionnaire de connexions de fichiers plats après l'ajout et la configuration de la source de fichiers plats qui utilise le gestionnaire de connexions, il n'est pas nécessaire de redimensionner manuellement les colonnes de sortie dans la source de fichiers plats. Quand vous ouvrez la boîte de dialogue **Source du fichier plat** , la source du fichier plat offre la possibilité de synchroniser les métadonnées de la colonne.  
  
## <a name="configuration-of-the-flat-file-connection-manager"></a>Configuration du gestionnaire de connexions de fichiers plats  
 Quand vous ajoutez un gestionnaire de connexions de fichiers plats à un package, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] crée un gestionnaire de connexions qui sera résolu en une connexion de fichiers plats au moment de l’exécution, définit les propriétés de connexion de fichiers plats et ajoute le gestionnaire de connexions de fichiers plats à la collection **Connections** du package.  
  
 La propriété **ConnectionManagerType** du gestionnaire de connexions a pour valeur **FLATFILE**.  
  
 Par défaut, le gestionnaire de connexions de fichiers plats cherche toujours la présence d'un séparateur de lignes dans les données non délimitées par des guillemets, puis démarre une nouvelle ligne lorsqu'un séparateur de lignes est trouvé. Cela permet au gestionnaire de connexions de fichiers plats d'analyser correctement les fichiers comportant des lignes auxquelles il manque des champs de colonnes.  
  
 Dans certains cas, la désactivation de cette fonctionnalité peut améliorer les performances des packages. Vous pouvez désactiver cette fonctionnalité en affectant à la propriété **AlwaysCheckForRowDelimiters**du gestionnaire de connexions de fichiers plats la valeur **False**.  
  
 Vous pouvez configurer le gestionnaire de connexions de fichiers plats de plusieurs manières :  
  
-   Spécifiez le fichier, les paramètres régionaux et la page de codes à utiliser. Les paramètres régionaux sont utilisés pour interpréter les données spécifiques à un pays comme les dates, tandis que la page de codes est utilisée pour convertir les données chaînes au format Unicode.  
  
-   Spécifiez le format de fichier. Vous pouvez utiliser un format délimité, à largeur fixe ou en drapeau à droite.  
  
-   Spécifiez une ligne d'en-tête, une ligne de données et des séparateurs de colonnes. Les séparateurs de colonnes peuvent être définis au niveau du fichier et remplacés au niveau de la colonne.  
  
-   Indiquez si la première ligne du fichier contient les noms de colonnes.  
  
-   Spécifiez un caractère d'identificateur de texte. Chaque colonne peut être configurée pour reconnaître un identificateur de texte.  
  
     L’utilisation d’un caractère qualificateur pour incorporer un caractère qualificateur dans une chaîne qualifiée est désormais prise en charge par le gestionnaire de connexions de fichiers plats. La double instance d'un qualificateur de texte est interprétée comme une instance littérale et unique de cette chaîne. Par exemple, si l'identificateur de texte est un guillemet simple et si les données d'entrée sont 'abc', 'def', 'g'hi', les données de sortie sont abc, def, g'hi. Toutefois, une instance d’un qualificateur incorporé dans une chaîne qualifiée provoque l’échec de la source du fichier plat avec l’erreur DTS_E_PRIMEOUTPUTFAILED.
  
-   Définissez des propriétés comme le nom, le type de données et la largeur maximale pour des colonnes individuelles.  
  
 Vous pouvez définir la propriété ConnectionString du gestionnaire de connexions de fichiers plats en spécifiant une expression dans la fenêtre Propriétés de [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]. Pour éviter une erreur de validation, procédez comme suit.  
  
-   Quand vous utilisez une expression pour spécifier le fichier, ajoutez un chemin de fichier dans la zone **Nom du fichier** de **l’Éditeur du gestionnaire de connexions de fichiers plats**.  
  
-   Attribuez à la propriété **DelayValidation** du gestionnaire de connexions de fichiers plats la valeur **True**.  
  
 Vous pouvez utiliser une expression pour créer un nom de fichier au moment de l'exécution à l'aide du gestionnaire de connexions de fichiers plats, avec la destination du fichier plat.  
  
 Vous pouvez définir les propriétés par le biais du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] ou par programmation.  
  
 Pour plus d’informations sur la configuration d’un gestionnaire de connexions par programmation, consultez <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager> et [Ajout de connexions par programme](../../integration-services/building-packages-programmatically/adding-connections-programmatically.md).  
  
## <a name="flat-file-connection-manager-editor-general-page"></a>Éditeur du gestionnaire de connexions de fichiers plats (page Général)
  La page **Général** de la boîte de dialogue **Éditeur du gestionnaire de connexions de fichiers plats** vous permet de sélectionner un format de fichier et de données. Une connexion de fichier plat permet à un package de se connecter à un fichier texte.  
  
 Pour en savoir plus sur le gestionnaire de connexions de fichiers plats, consultez [Flat File Connection Manager](../../integration-services/connection-manager/flat-file-connection-manager.md).  
  
### <a name="options"></a>Options  
 **Nom du gestionnaire de connexions**  
 Permet de fournir un nom unique pour la connexion de fichier plat dans le flux de travail. Le nom fourni sera affiché dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] .  
  
 **Description**  
 Décrit la connexion. Il est recommandé d'indiquer ici l'usage auquel la connexion est destinée, de sorte que les packages soient correctement documentés et plus faciles à gérer.  
  
 **Nom de fichier**  
 Tapez le chemin d'accès et le nom de fichier à utiliser dans la connexion de fichier plat.  
  
 **Parcourir**  
 Recherchez le nom de fichier à utiliser dans la connexion de fichier plat.  
  
 **Paramètres régionaux**  
 Spécifiez les paramètres régionaux pour fournir les informations spécifiques à la langue relatives à l'ordre et aux formats de date et d'heure.  
  
 **Unicode**  
 Indique s'il convient d'utiliser Unicode. Si vous utilisez Unicode, vous ne pouvez pas spécifier de page de codes.  
  
 **Page de codes**  
 Spécifiez la page de codes pour du texte non-Unicode.  
  
 **Format**  
 Indique si le fichier utilise une mise en forme délimitée, à largeur fixe ou en drapeau à droite.  
  
|Valeur|Description|  
|-----------|-----------------|  
|Délimité|Les colonnes sont séparées par les séparateurs spécifiés à la page **Colonnes** .|  
|Largeur fixe|Les colonnes ont une largeur fixe.|  
|En drapeau à droite|Les fichiers en drapeau à droite sont des fichiers dans lesquels chaque colonne a une largeur fixe, à l'exception de la dernière colonne. Un séparateur de lignes s'applique.|  
  
 **Qualificateur de texte**  
 Spécifiez le qualificateur de texte à utiliser. Par exemple, vous pouvez spécifier que les champs de texte soient placés entre guillemets.  
  
> [!NOTE]  
>  Après avoir sélectionné un identificateur de texte, vous ne pouvez pas sélectionner de nouveau l’option **Aucun** . Tapez **Aucun** pour désélectionner l’identificateur de texte.  
  
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
 Spécifiez le nombre de lignes d'en-tête ou de lignes de données initiales à ignorer, le cas échéant.  
  
 **Noms de colonnes dans la première ligne de données**  
 Indique si des noms de colonne doivent être attendus ou fournis dans la première ligne de données.  
## <a name="flat-file-connection-manager-editor-columns-page"></a>Éditeur du gestionnaire de connexions de fichiers plats (page Colonnes)
  La page **Colonnes** de la boîte de dialogue **Éditeur du gestionnaire de connexions de fichiers plats** vous permet de spécifier les informations de ligne et de colonne, ainsi que d'afficher un aperçu du fichier.  
  
 Pour en savoir plus sur le gestionnaire de connexions de fichiers plats, consultez [Flat File Connection Manager](../../integration-services/connection-manager/flat-file-connection-manager.md).  
  
### <a name="static-options"></a>Options statiques  
 **Nom du gestionnaire de connexions**  
 Fournit un nom unique pour la connexion de fichiers plats du flux de travail. Le nom fourni sera affiché dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] .  
  
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
  
 **Actualiser**  
 Affichez les résultats des modifications des séparateurs en cliquant sur **Actualiser**. Il ne devient visible qu'après avoir changé d'autres options de connexion.  
  
 **Aperçu des lignes**  
 Affichez les données d'exemple dans le fichier plat, divisées en colonnes et en lignes à l'aide des options sélectionnées.  
  
 **Réinitialiser les colonnes**  
 Cliquez sur **Réinitialiser les colonnes**pour supprimer toutes les colonnes à l’exception de celles d’origine.  
  
#### <a name="format--fixed-width"></a>Format = Largeur fixe  
 **Police**  
 Sélectionnez la police d'affichage de l'aperçu des données.  
  
 **Colonnes de données sources**  
 Ajustez la largeur de la ligne en faisant glisser le marqueur de ligne rouge vertical, et celle des colonnes en cliquant sur la règle en haut de la fenêtre d'aperçu.  
  
 **Largeur de ligne**  
 Spécifiez la largeur de la ligne avant d'ajouter des séparateurs pour des colonnes distinctes. Vous pouvez également faire glisser la ligne rouge verticale dans la fenêtre d'aperçu pour marquer la fin de la ligne. La valeur de la largeur de ligne est automatiquement mise à jour.  
  
 **Réinitialiser les colonnes**  
 Cliquez sur **Réinitialiser les colonnes**pour supprimer toutes les colonnes à l’exception de celles d’origine.  
  
#### <a name="format--ragged-right"></a>Format = En drapeau à droite  
  
> [!NOTE]  
>  Les fichiers en drapeau à droite sont des fichiers dans lesquels chaque colonne a une largeur fixe, à l'exception de la dernière colonne. Un séparateur de lignes s'applique.  
  
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
 Cliquez sur **Réinitialiser les colonnes**pour supprimer toutes les colonnes à l’exception de celles d’origine.  
## <a name="flat-file-connection-manager-editor-advanced-page"></a>Éditeur du gestionnaire de connexions de fichiers plats (page Avancé)
  La page **Avancé** de la boîte de dialogue **Éditeur du gestionnaire de connexions de fichiers plats** vous permet de définir des propriétés qui spécifient la manière dont Integration Services lit et écrit des données dans les fichiers plats. Vous pouvez modifier les noms des colonnes dans le fichier plat et définir des propriétés qui incluent le type de données et des séparateurs pour chaque colonne du fichier.  
  
 Par défaut, la longueur des colonnes de type chaîne est de 50 caractères. Vous pouvez redimensionner la longueur de ces colonnes pour empêcher la troncation des données ou une largeur de colonne excessive. Vous pouvez également mettre à jour d'autres métadonnées pour permettre la compatibilité avec les colonnes de destination. Par exemple, vous pouvez convertir le type de données d'une colonne qui contient uniquement des données de type entier en type de données numérique, tel que DT_I2. Vous pouvez apporter ces modifications manuellement ou cliquer sur le bouton **Sélectionner les types** pour utiliser la boîte de dialogue **Suggérer les types de colonnes** pour évaluer des échantillons de données et effectuer automatiquement une partie de ces modifications.  
  
 Pour en savoir plus sur le gestionnaire de connexions de fichiers plats, consultez [Flat File Connection Manager](../../integration-services/connection-manager/flat-file-connection-manager.md).  
  
### <a name="options"></a>Options  
 **Nom du gestionnaire de connexions**  
 Spécifiez un nom unique pour le gestionnaire de connexions de fichiers plats dans le flux de travail. Le nom fourni sera affiché dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] .  
  
 **Description**  
 Décrivez le gestionnaire de connexions. Il est recommandé d'indiquer ici l'usage auquel le gestionnaire de connexions est destiné, de sorte que les packages soient correctement documentés et plus faciles à gérer.  
  
 **Configurez les propriétés de chaque colonne**  
 Dans le volet gauche, sélectionnez une colonne pour afficher ses propriétés dans celui de droite. Le tableau ci-dessous fournit une description des définitions des types de données. Certaines de ces propriétés peuvent uniquement être configurées pour certains formats de fichiers plats.  
  
|Propriété|Description|  
|--------------|-----------------|  
|**ColumnType**|Indique si la colonne est délimitée, si elle a une largeur fixe ou si elle présente un format en drapeau à droite. Cette propriété est en lecture seule. Les fichiers en drapeau à droite sont des fichiers dans lesquels chaque colonne a une largeur fixe, à l'exception de la dernière colonne. Un séparateur de lignes s'applique.|  
|**OutputColumnWidth**|Indiquez une valeur spécifiant la largeur de colonne en nombre d'octets. Pour les fichiers Unicode, cette valeur correspond à un nombre de caractères. Dans la tâche de flux de données, cette valeur permet de définir la largeur de la colonne de sortie pour les fichiers plats sources. Dans le modèle objet, le nom de la propriété est MaximumWidth.|  
|**DataType**|Sélectionnez un type de données dans la liste des types de données disponibles. Pour plus d'informations, consultez [Integration Services Data Types](../../integration-services/data-flow/integration-services-data-types.md).|  
|**TextQualified**|Indique si les données de texte sont entourées par des caractères identificateurs de texte, tels que des caractères de guillemets.<br /><br /> True : les données texte du fichier plat sont qualifiées. False : les données texte du fichier plat ne sont pas qualifiées.|  
|**Nom**|Précisez un nom de colonne descriptif. Si vous n'entrez aucun nom, [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] crée automatiquement un nom au format Colonne 0, Colonne 1, et ainsi de suite.|  
|**DataScale**|Spécifiez l'échelle des données numériques. L'échelle est le nombre de décimales. Pour plus d'informations, consultez [Integration Services Data Types](../../integration-services/data-flow/integration-services-data-types.md).|  
|**ColumnDelimiter**|Sélectionnez un délimiteur de colonnes dans la liste des séparateurs de colonnes disponibles. Veillez à choisir un caractère de séparation qu'il est peu probable de rencontrer dans le texte. Cette valeur est ignorée dans le cas des colonnes à largeur fixe.<br /><br /> **{CR}{LF}**. Les colonnes sont délimitées par une combinaison retour chariot-saut de ligne.<br /><br /> **{CR}**. Les colonnes sont séparées par un retour chariot.<br /><br /> **{LF}**. Les colonnes sont séparées par un saut de ligne.<br /><br /> **Point-virgule {;}**. Les colonnes sont séparées par un point-virgule.<br /><br /> **Deux-points {:}**. Les colonnes sont séparées par un deux-points.<br /><br /> **Virgule {,}**. Les colonnes sont séparées par une virgule.<br /><br /> **Tabulation {t}**. Les colonnes sont séparées par une tabulation.<br /><br /> **Barre verticale {&#124;}**. Les colonnes sont séparées par une barre verticale.|  
|**DataPrecision**|Spécifiez la précision des données numériques. La précision indique le nombre total de chiffres. Pour plus d'informations, consultez [Integration Services Data Types](../../integration-services/data-flow/integration-services-data-types.md).|  
|**InputColumnWidth**|Indiquez une valeur spécifiant la largeur de colonne en nombre d'octets. Pour les fichiers Unicode, cette valeur est exprimée en nombre de caractères. Cette valeur est ignorée dans le cas des colonnes délimitées.<br /><br /> **Remarque** : Dans le modèle objet, le nom de cette propriété est ColumnWidth.|  
  
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
 La boîte de dialogue **Suggérer les types de colonnes** permet d’évaluer des échantillons de données dans le fichier et d’obtenir des suggestions pour le type de données et la longueur de chaque colonne. Pour plus d’informations, consultez [Référence de l’interface utilisateur de la boîte de dialogue Suggérer les types de colonnes](../../integration-services/connection-manager/suggest-column-types-dialog-box-ui-reference.md).  
## <a name="flat-file-connection-manager-editor-preview-page"></a>Éditeur du gestionnaire de connexions de fichiers plats (Enterprise Manager)
  Utilisez le nœud **Aperçu** de la boîte de dialogue **Éditeur du gestionnaire de connexions de fichiers plats** pour visualiser le contenu du fichier source sous forme de tableau.  
  
 Pour en savoir plus sur le gestionnaire de connexions de fichiers plats, consultez [Flat File Connection Manager](../../integration-services/connection-manager/flat-file-connection-manager.md).  
  
### <a name="options"></a>Options  
 **Nom du gestionnaire de connexions**  
 Fournit un nom unique pour la connexion de fichiers plats du flux de travail. Le nom fourni sera affiché dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] .  
  
 **Description**  
 Décrit la connexion. Il est recommandé d'indiquer ici l'usage auquel la connexion est destinée, de sorte que les packages soient correctement documentés et plus faciles à gérer.  
  
 **Lignes de données à ignorer**  
 Indiquez le nombre de lignes qui doivent être ignorées à partir du début du fichier plat.  
  
 **Actualiser**  
 Cliquez sur le bouton **Actualiser**pour visualiser l’effet obtenu en modifiant le nombre de lignes à ignorer. Il ne devient visible qu'après avoir changé d'autres options de connexion.  
  
 **Aperçu des lignes**  
 Affiche un échantillon de données du fichier plat, divisé en colonnes et en lignes en fonction des options que vous avez sélectionnées.  
 
