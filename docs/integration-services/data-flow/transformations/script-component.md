---
title: Composant Script | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: data-flow
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.scriptcomponentdetails.f1
- sql13.dts.designer.scriptcomponent.f1
- sql13.dts.designer.scriptcomponent.connections.f1
- sql13.dts.designer.scriptcomponent.inputcolumn.f1
- sql13.dts.designer.scriptcomponent.columnproperties.f1
- sql13.dts.designer.scriptcomponent.script.f1
helpviewer_keywords:
- Script transformation
- scripts [Integration Services], transformations
- Script component [Integration Services], about Script component
- Script component [Integration Services]
ms.assetid: 131c2d0c-2e33-4785-94af-ada5c049821e
caps.latest.revision: 70
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 28b0e0036e6c85e6898c4d2f24007eb8915b3703
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="script-component"></a>Composant Script
  Le composant Script héberge le script et permet à un package d'inclure du code de script personnalisé et de l'exécuter. Vous pouvez utiliser le composant Script dans des packages pour :  
  
-   Appliquer plusieurs transformations aux données, au lieu d'utiliser plusieurs transformations dans le flux de données. Par exemple, un script peut ajouter les valeurs de deux colonnes, puis calculer la moyenne de la somme.  
  
-   Accéder aux règles métier dans un assembly .NET existant. Par exemple, un script peut appliquer une règle métier qui spécifie la plage de valeurs valides dans une colonne **Income** .  
  
-   Utiliser des formules et des fonctions personnalisées en plus des fonctions et des opérateurs fournis par la grammaire des expressions [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] . Par exemple, validez les numéros de carte de crédit qui utilisent la formule LUHN.  
  
-   Valider les données des colonnes et ignorer les enregistrements contenant des données non valides. Par exemple, un script peut évaluer un montant de frais de port raisonnable et ignorer les enregistrements présentant des montants extrêmement élevés ou bas.  
  
 Le composant Script est un moyen simple et rapide d'inclure des fonctions personnalisées dans un flux de données. Néanmoins, si vous envisagez de réutiliser le code de script dans plusieurs packages, vous devez envisager de programmer un composant personnalisé au lieu d'utiliser le composant Script. Pour plus d’informations, consultez [Développement d’un composant de flux de données personnalisé](../../../integration-services/extending-packages-custom-objects/data-flow/developing-a-custom-data-flow-component.md).  
  
> [!NOTE]  
>  Si le composant Script contient un script qui essaie de lire la valeur d'une colonne qui est NULL, il échoue lorsque vous exécutez le package. Il est recommandé que votre script utilise la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptBuffer.IsNull%2A> pour déterminer si la colonne est NULL avant d’essayer de lire la valeur de la colonne.  
  
 Le composant Script peut être utilisé en tant que source, transformation ou destination. Il prend en charge une entrée et plusieurs sorties. En fonction de l'utilisation qui est faite du composant, il prendra en charge une entrée ou des sorties, ou bien les deux. Le script est appelé par chaque ligne de l'entrée ou de la sortie.  
  
-   Utilisé comme source, le composant Script prend en charge plusieurs sorties.  
  
-   Utilisé comme transformation, le composant Script prend en charge une entrée et plusieurs sorties.  
  
-   Utilisé comme destination, le composant Script prend en charge une entrée.  
  
 Le composant Script ne prend pas en charge les affichages des erreurs.  
  
 Lorsque vous avez décidé que le composant Script est le choix approprié pour votre package, vous devez configurer les entrées et sorties, développer le script que le composant utilise et configurer le composant lui-même.  
  
## <a name="understanding-the-script-component-modes"></a>Présentation des modes du composant Script  
 Dans le concepteur [!INCLUDE[ssIS](../../../includes/ssis-md.md)] , le composant Script propose deux modes : le mode Création des métadonnées et le mode Création du code. En mode Création des métadonnées, vous pouvez ajouter et modifier les entrées et les sorties du composant Script, mais vous ne pouvez pas écrire de code. Une fois toutes les entrées et sorties configurées, vous devez basculer en mode Création du code afin d'écrire le script. Le composant Script génère automatiquement le code de base à partir des métadonnées des entrées et des sorties. Si vous modifiez les métadonnées une fois que le composant Script a généré le code de base, votre code risque de ne plus se compiler, car le code de base compilé mis à jour sera peut-être incompatible avec votre code.  
  
## <a name="writing-the-script-that-the-component-uses"></a>Écriture du script utilisé par le composant  
 Le composant Script utilise [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)] Tools for Applications (VSTA) comme environnement d’écriture des scripts. Vous pouvez accéder à VSTA à partir de **l’Éditeur de transformation de script**. Pour plus d’informations, consultez [Éditeur de transformation de script &#40;page Script&#41;](../../../integration-services/data-flow/transformations/script-transformation-editor-script-page.md).  
  
 Le composant Script propose un projet VSTA qui inclut une classe auto-générée, nommée ScriptMain, qui représente les métadonnées du composant. Par exemple, si le composant Script est utilisé en tant que transformation avec trois sorties, ScriptMain inclut une méthode pour chaque sortie. ScriptMain est le point d'entrée dans le script.  
  
 VSTA contient l'ensemble des fonctionnalités standard de l'environnement [!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)] , comme l'éditeur [!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)] à code de couleur, IntelliSense et l'Explorateur d'objets. Le script utilisé par le composant Script est stocké dans la définition du package. Quand vous concevez le package, le code de script est écrit temporairement dans un fichier projet.  
  
 VSTA prend en charge les langages de programmation [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Visual C# et [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Visual Basic.  
  
 Pour plus d’informations sur la programmation du composant Script, consultez [Extension du flux de données avec le composant Script](../../../integration-services/extending-packages-scripting/data-flow-script-component/extending-the-data-flow-with-the-script-component.md). Pour obtenir des informations spécifiques sur la configuration du composant Script en tant que source, transformation ou destination, consultez [Développement de types spécifiques de composants Script](../../../integration-services/extending-packages-scripting-data-flow-script-component-types/developing-specific-types-of-script-components.md). Pour obtenir d’autres exemples, notamment une destination ODBC, démontrant l’utilisation du composant Script, consultez [Exemples supplémentaires du composant Script](../../../integration-services/extending-packages-scripting-data-flow-script-component-examples/additional-script-component-examples.md).  
  
> [!NOTE]  
>  Contrairement aux versions antérieures pour lesquelles il était possible d’indiquer si les scripts étaient précompilés, tous les scripts de [!INCLUDE[ssISversion10](../../../includes/ssisversion10-md.md)] et des versions ultérieures sont précompilés. Lorsqu'un script est précompilé, le moteur de langage n'est pas chargé au moment de l'exécution et le package s'exécute plus rapidement. Toutefois, les fichiers binaires précompilés occupent un espace disque important.  
  
## <a name="configuring-the-script-component"></a>Configuration du composant Script  
 Vous pouvez configurer le composant Script de plusieurs manières :  
  
-   Sélectionnez les colonnes d'entrée à référencer.  
  
    > [!NOTE]  
    >  Vous pouvez configurer une seule entrée quand vous utilisez le concepteur [!INCLUDE[ssIS](../../../includes/ssis-md.md)] .  
  
-   Indiquez le script exécuté par le composant.  
  
-   Spécifiez le langage de script  
  
-   Fournissez des listes de variables en lecture seule et en lecture/écrite séparées par des virgules.  
  
-   Ajoutez des sorties supplémentaires, ainsi que des colonnes de sortie auxquelles le script affecte des valeurs.  
  
 Vous pouvez définir les propriétés par le biais du concepteur [!INCLUDE[ssIS](../../../includes/ssis-md.md)] ou par programmation.  
  
### <a name="configuring-the-script-component-in-the-designer"></a>Configuration du composant Script dans le concepteur  
 Pour plus d'informations sur la définition de ces propriétés dans le concepteur [!INCLUDE[ssIS](../../../includes/ssis-md.md)] , cliquez sur la rubrique suivante :  
  
-   [Définir les propriétés d'un composant de flux de données](../../../integration-services/data-flow/set-the-properties-of-a-data-flow-component.md)  
  
### <a name="configuring-the-script-component-programmatically"></a>Configuration du composant Script par programmation  
 Pour plus d’informations sur les propriétés définissables dans la fenêtre **Propriétés** ou par programmation, cliquez sur l’une des rubriques suivantes :  
  
-   [Propriétés communes](http://msdn.microsoft.com/library/51973502-5cc6-4125-9fce-e60fa1b7b796)  
  
-   [Propriétés personnalisées des transformations](../../../integration-services/data-flow/transformations/transformation-custom-properties.md)  
  
 Pour plus d'informations sur la définition des propriétés, cliquez sur l'une des rubriques suivantes :  
  
-   [Définir les propriétés d’un composant de flux de données](../../../integration-services/data-flow/set-the-properties-of-a-data-flow-component.md)  
  
## <a name="select-script-component-type"></a>Sélectionner le type de composant de script
  La boîte de dialogue **Sélectionner le type de composant de script** permet d'indiquer s'il est nécessaire de créer une transformation de script préconfigurée pour servir de source, de transformation ou de destination.  
  
 Pour en savoir plus sur le composant Script, consultez [Configuration du composant Script dans l’éditeur de composant de script](../../../integration-services/extending-packages-scripting/data-flow-script-component/configuring-the-script-component-in-the-script-component-editor.md). Pour en savoir plus sur la programmation du composant de script, consultez [Extending the Data Flow with the Script Component](../../../integration-services/extending-packages-scripting/data-flow-script-component/extending-the-data-flow-with-the-script-component.md).  
  
### <a name="options"></a>Options  
 Selon que vous choisissez **Source**, **Destination**ou **Transformation** , cela affecte la configuration de la transformation de script ainsi que les pages de l'Éditeur de transformation de script.  
  
## <a name="script-transformation-editor-connection-managers-page"></a>Éditeur de transformation de script (Page Gestionnaire de connexions)
  La page **Gestionnaire de connexions** de **l’Éditeur de transformation de script** permet de spécifier les connexions que doit utiliser le script.  
  
 Pour en savoir plus sur le composant Script, consultez [Configuration du composant Script dans l’éditeur de composant de script](../../../integration-services/extending-packages-scripting/data-flow-script-component/configuring-the-script-component-in-the-script-component-editor.md). Pour en savoir plus sur la programmation du composant de script, consultez [Extending the Data Flow with the Script Component](../../../integration-services/extending-packages-scripting/data-flow-script-component/extending-the-data-flow-with-the-script-component.md).  
  
### <a name="options"></a>Options  
 **Connection managers**  
 Affiche la liste des connexions disponibles que peut utiliser le script.  
  
 **Nom**  
 Tapez un nom unique descriptif pour la connexion.  
  
 **Gestionnaire de connexions**  
 Effectuez une sélection dans la liste des gestionnaires de connexions disponibles ou sélectionnez **\<Nouvelle connexion>** pour ouvrir la boîte de dialogue **Ajout d’un gestionnaire de connexions SSIS**.  
  
 **Description**  
 Tapez une description pour la connexion.  
  
 **Ajouter**  
 Ajoute une connexion à la liste **Gestionnaires de connexions** .  
  
 **Supprimer**  
 Supprime la connexion sélectionnée de la liste **Gestionnaires de connexions** .  
  
## <a name="script-transformation-editor-input-columns-page"></a>Éditeur de transformation de script (page Colonnes d'entrée)
  Utilisez la page **Colonnes d’entrée** de la boîte de dialogue **Éditeur de transformation de script** pour définir les propriétés des colonnes d’entrée.  
  
> [!NOTE]  
>  La page **Colonnes d’entrée** ne s’affiche pas pour les composants source qui ont des sorties, mais pas d’entrées.  
  
 Pour en savoir plus sur le composant Script, consultez [Configuration du composant Script dans l’éditeur de composant de script](../../../integration-services/extending-packages-scripting/data-flow-script-component/configuring-the-script-component-in-the-script-component-editor.md). Pour en savoir plus sur la programmation du composant de script, consultez [Extending the Data Flow with the Script Component](../../../integration-services/extending-packages-scripting/data-flow-script-component/extending-the-data-flow-with-the-script-component.md).  
  
### <a name="options"></a>Options  
 **Nom de l'entrée**  
 Sélectionnez le nom d'une entrée dans la liste.  
  
 **Colonnes d'entrée disponibles**  
 Utilisez les cases à cocher pour spécifier les colonnes que la transformation de script utilisera.  
  
 **Colonne d'entrée**  
 Sélectionnez dans la liste le nom d'une colonne d'entrée pour chaque ligne. Vos sélections se reflètent dans les sélections des cases à cocher de la table **Colonnes d’entrée disponibles**.  
  
 **Alias de sortie**  
 Permet de saisir un alias pour chaque colonne de sortie. Par défaut, il s'agit du nom de la colonne d'entrée ; vous pouvez néanmoins choisir un nom unique et descriptif.  
  
 **Type d'utilisation**  
 Spécifiez si la transformation de script traite chaque colonne en **ReadOnly** ou en **ReadWrite**.  
  
## <a name="script-transformation-editor-inputs-and-outputs-page"></a>Éditeur de transformation de script (page Entrées et sorties)
  Utilisez la page **Entrées et sorties** de la boîte de dialogue **Éditeur de transformation de script** pour ajouter, supprimer et configurer les entrées et les sorties destinées à la transformation de script.  
  
> [!NOTE]  
>  Les composants source disposent de sorties mais d'aucune entrée ; à l'inverse, les composants de destination possèdent des entrées mais aucune sortie. Les transformations, elles, ont les deux.  
  
 Pour en savoir plus sur le composant Script, consultez [Configuration du composant Script dans l’éditeur de composant de script](../../../integration-services/extending-packages-scripting/data-flow-script-component/configuring-the-script-component-in-the-script-component-editor.md). Pour en savoir plus sur la programmation du composant de script, consultez [Extending the Data Flow with the Script Component](../../../integration-services/extending-packages-scripting/data-flow-script-component/extending-the-data-flow-with-the-script-component.md).  
  
### <a name="options"></a>Options  
 **Inputs and outputs**  
 Permet de sélectionner une entrée ou une sortie à gauche pour en voir les propriétés dans le tableau de droite. Les propriétés pouvant être modifiées varient en fonction de la sélection faite. La plupart des propriétés affichées sont en lecture seule. Pour plus d'informations sur chacune de ces propriétés, consultez les rubriques suivantes.  
  
 [Propriétés communes](http://msdn.microsoft.com/library/51973502-5cc6-4125-9fce-e60fa1b7b796)  
  
 [Propriétés personnalisées des transformations](../../../integration-services/data-flow/transformations/transformation-custom-properties.md)  
  
 **Ajouter une sortie**  
 Ajoute une sortie à la liste.  
  
 **Ajouter une colonne**  
 Sélectionnez un dossier dans lequel placer la nouvelle colonne de sortie, puis ajoutez la colonne en cliquant sur **Ajouter une colonne**.  
  
 **Supprimer une sortie**  
 Sélectionnez une sortie, puis supprimez-la en cliquant sur **Supprimer une sortie**.  
  
 **Supprimer une colonne**  
 Sélectionnez une colonne, puis cliquez sur le bouton **Supprimer une colonne**pour la supprimer.  
  
## <a name="script-transformation-editor-script-page"></a>Éditeur de transformation de script (page Script)
  Utilisez l'onglet **Script** de la boîte de dialogue **Éditeur de transformation de script** pour définir un script et les propriétés associées.  
  
 Pour en savoir plus sur le composant Script, consultez [Configuration du composant Script dans l’éditeur de composant de script](../../../integration-services/extending-packages-scripting/data-flow-script-component/configuring-the-script-component-in-the-script-component-editor.md). Pour en savoir plus sur la programmation du composant de script, consultez [Extending the Data Flow with the Script Component](../../../integration-services/extending-packages-scripting/data-flow-script-component/extending-the-data-flow-with-the-script-component.md).  
  
### <a name="options"></a>Options  
 **Propriétés**  
 Affichez et modifiez les propriétés de la transformation de script. La plupart des propriétés affichées sont en lecture seule. Vous pouvez modifier les propriétés suivantes :  
  
|Valeur|Description|  
|-----------|-----------------|  
|**Description**|Décrit la transformation de script en terme de fonction.|  
|**LocaleID**|Définissez les paramètres régionaux pour fournir des informations spécifiques à la région relatives au tri et à la conversion de date et d'heure.|  
|**Nom**|Entrez un nom descriptif pour le composant.|  
|**ValidateExternalMetadata**|Indiquez si la transformation de script valide les métadonnées de colonne par rapport aux sources de données externes lors de la conception. La valeur **false** diffère la validation jusqu'à l'exécution.|  
|**ReadOnlyVariables**|Tapez une liste de variables séparées par une virgule pour l'accès en lecture seule par la transformation de script.<br /><br /> Remarque : les noms des variables respectent la casse.|  
|**ReadWriteVariables**|Tapez une liste de variables séparées par une virgule pour l'accès en lecture/écriture par la transformation de script.<br /><br /> Remarque : les noms des variables respectent la casse.|  
|**ScriptLanguage**|Sélectionnez le langage de script que le composant de script doit utiliser.<br /><br /> Pour définir le langage de script par défaut pour les composants et les tâches de script, utilisez l'option **Langage de script** dans la page **Général** de la boîte de dialogue **Options** .|  
|**UserComponentTypeName**|Spécifie la classe <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponentHost> et l’assembly **Microsoft.SqlServer.TxScript** qui prennent en charge l’infrastructure [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .|  
  
 **Modifier le script**  
 Utilisez [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)] Tools for Applications (VSTA) pour créer ou modifier un script.  
  
## <a name="related-content"></a>Contenu associé  
 [Transformations Integration Services](../../../integration-services/data-flow/transformations/integration-services-transformations.md)  
  
 [Extension du flux de données avec le composant Script](../../../integration-services/extending-packages-scripting/data-flow-script-component/extending-the-data-flow-with-the-script-component.md)  
  
  
