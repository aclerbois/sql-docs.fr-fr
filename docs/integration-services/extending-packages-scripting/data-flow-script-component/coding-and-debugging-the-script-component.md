---
title: Codage et débogage du composant Script | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: extending-packages-scripting
ms.reviewer: ''
ms.suite: sql
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: reference
applies_to:
- SQL Server 2016 Preview
dev_langs:
- VB
helpviewer_keywords:
- SSIS Script task, development environment
- Script component [Integration Services], debugging
- Script component [Integration Services], coding
- SSIS Script component, debugging
- Script component [Integration Services], development environment
- debugging [Integration Services], scripts
- SSIS Script component, coding
- VSTA
ms.assetid: c3913c15-66aa-4b61-89b5-68488fa5f0a4
caps.latest.revision: 66
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 0d45c217f4a7f6ed93f6688f931362339859d058
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="coding-and-debugging-the-script-component"></a>Codage et débogage du composant Script
  Dans le Concepteur [!INCLUDE[ssIS](../../../includes/ssis-md.md)], le composant Script propose deux modes : le mode Création des métadonnées et le mode Création du code. Lorsque vous ouvrez l’**Éditeur de transformation de script**, le composant entre en mode Création des métadonnées, dans lequel vous configurez des métadonnées et définissez les propriétés du composant. Après avoir défini les propriétés du composant Script et avoir configuré l'entrée et les sorties en mode Création des métadonnées, vous pouvez basculer vers le mode Création du code pour écrire votre script personnalisé. Pour plus d’informations sur le mode Création des métadonnées et le mode Création de code, consultez [Configuration du composant Script dans l’Éditeur de composant Script](../../../integration-services/extending-packages-scripting/data-flow-script-component/configuring-the-script-component-in-the-script-component-editor.md).  
  
## <a name="writing-the-script-in-code-design-mode"></a>Écriture du script en mode Création du code  
  
### <a name="script-component-development-environment"></a>Environnement de développement du composant Script  
 Pour écrire votre script, cliquez sur **Modifier le script** dans la page **Script** de l’**Éditeur de transformation de script** afin d’ouvrir l’environnement de développement intégré [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)] Tools for Applications (VSTA). L'environnement de développement intégré VSTA contient l'ensemble des fonctionnalités standard de l'environnement [!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)] .NET, telles que l'éditeur [!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)] à code de couleurs, IntelliSense et l'Explorateur d'objets.  
  
 Le code de script est écrit dans [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Visual Basic ou [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Visual C#. Vous spécifiez le langage de script en définissant la propriété **ScriptLanguage** dans l’**Éditeur de transformation de script**. Si vous préférez utiliser un autre langage de programmation, vous pouvez développer un assembly personnalisé dans le langage de votre choix et appeler ses fonctionnalités à partir du code inclus dans le composant Script.  
  
 Le script que vous créez dans le composant Script est stocké dans la définition du package. Aucun fichier de script distinct n'est créé. Par conséquent, l'utilisation du composant Script n'affecte pas le déploiement du package.  
  
> [!NOTE]  
>  Lorsque vous concevez le package, le code de script est écrit temporairement dans un fichier projet. Étant donné que le stockage d'informations sensibles dans un fichier représente un risque potentiel en termes de sécurité, nous vous recommandons de ne pas inclure d'informations sensibles telles que des mots de passe dans le code de script.  
  
 Par défaut, **Option Strict** est désactivé dans l’environnement de développement intégré.  
  
### <a name="script-component-project-structure"></a>Structure du projet de composant Script  
 La force du composant Script réside dans sa capacité à générer un code d'infrastructure qui réduit la quantité de code à écrire. Cette fonctionnalité repose sur le fait que les entrées et sorties et leurs colonnes et propriétés sont fixes et connues à l'avance. Par conséquent, toutes les modifications ultérieures que vous apportez aux métadonnées du composant peuvent invalider le code que vous avez écrit. Cela provoque des erreurs de compilation pendant l'exécution du package.  
  
#### <a name="project-items-and-classes-in-the-script-component-project"></a>Éléments et classe de projet dans le projet de composant Script  
 Lorsque vous basculez en mode Création du code, l’environnement de développement intégré VSTA s’ouvre et affiche l’élément de projet **ScriptMain**. L’élément de projet **ScriptMain** contient la classe **ScriptMain** modifiable, qui sert de point d’entrée pour le script et qui est l’endroit où vous écrivez votre code. Les éléments de code inclus dans la classe varient selon le langage de programmation que vous avez sélectionné pour la tâche de script.  
  
 Le projet de script contient deux autres éléments de projet en lecture seule générés automatiquement :  
  
-   L’élément de projet **ComponentWrapper** contient trois classes :  
  
    -   La classe **UserComponent**, qui hérite de <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent> et contient les méthodes et propriétés que vous utiliserez pour traiter les données et interagir avec le package. La classe **ScriptMain** hérite de la classe **UserComponent**.  
  
    -   Une classe de collection **Connections** qui contient des références aux connexions sélectionnées dans la page Gestionnaire de connexions de l’Éditeur de transformation de script.  
  
    -   Une classe de collection **Variables** qui contient des références aux variables entrées dans les propriétés **ReadOnlyVariable** et **ReadWriteVariables** de la page **Script** de l’**Éditeur de transformation de script**.  
  
-   L’élément de projet **BufferWrapper** contient une classe qui hérite de l’objet <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptBuffer> pour chaque entrée et sortie configurée dans la page **Entrées et sorties** de l’**Éditeur de transformation de script**. Chacune de ces classes contient des propriétés d'accesseur typées qui correspondent aux colonnes d'entrée et de sortie configurées, ainsi que des tampons de flux de données qui contiennent les colonnes.  
  
 Pour plus d’informations sur la manière d’utiliser ces objets, méthodes et propriétés, consultez [Présentation du modèle objet du composant Script](../../../integration-services/extending-packages-scripting/data-flow-script-component/understanding-the-script-component-object-model.md). Pour plus d’informations sur l’utilisation des méthodes et des propriétés de ces classes dans un type de composant Script particulier, consultez la section [Autres exemples de composants Script](../../../integration-services/extending-packages-scripting-data-flow-script-component-examples/additional-script-component-examples.md). Les rubriques d'exemples contiennent également des exemples de code complets.  
  
 Lorsque vous configurez le composant Script en tant que transformation, l’élément de projet **ScriptMain** contient le code généré automatiquement suivant. Le modèle de code fournit également une vue d'ensemble du composant Script, ainsi que des informations supplémentaires sur la récupération et la manipulation des objets SSIS, tels que les variables, les événements et les connexions.  
  
```vb  
' Microsoft SQL Server Integration Services Script Component  
' Write scripts using Microsoft Visual Basic 2008.  
' ScriptMain is the entry point class of the script.  
  
Imports System  
Imports System.Data  
Imports System.Math  
Imports Microsoft.SqlServer.Dts.Pipeline.Wrapper  
Imports Microsoft.SqlServer.Dts.Runtime.Wrapper  
  
<Microsoft.SqlServer.Dts.Pipeline.SSISScriptComponentEntryPointAttribute> _  
<CLSCompliant(False)> _  
Public Class ScriptMain  
    Inherits UserComponent  
  
    Public Overrides Sub PreExecute()  
        MyBase.PreExecute()  
        '  
        ' Add your code here for preprocessing or remove if not needed  
        '  
    End Sub  
  
    Public Overrides Sub PostExecute()  
        MyBase.PostExecute()  
        '  
        ' Add your code here for postprocessing or remove if not needed  
        ' You can set read/write variables here, for example:  
        ' Me.Variables.MyIntVar = 100  
        '  
    End Sub  
  
    Public Overrides Sub Input0_ProcessInputRow(ByVal Row As Input0Buffer)  
        '  
        ' Add your code here  
        '  
    End Sub  
  
End Class  
```  
  
```csharp  
/* Microsoft SQL Server Integration Services user script component  
*  Write scripts using Microsoft Visual C# 2008.  
*  ScriptMain is the entry point class of the script.*/  
  
using System;  
using System.Data;  
using Microsoft.SqlServer.Dts.Pipeline.Wrapper;  
using Microsoft.SqlServer.Dts.Runtime.Wrapper;  
  
[Microsoft.SqlServer.Dts.Pipeline.SSISScriptComponentEntryPointAttribute]  
public class ScriptMain : UserComponent  
{  
  
    public override void PreExecute()  
    {  
        base.PreExecute();  
        /*  
          Add your code here for preprocessing or remove if not needed  
        */  
    }  
  
    public override void PostExecute()  
    {  
        base.PostExecute();  
        /*  
          Add your code here for postprocessing or remove if not needed  
          You can set read/write variables here, for example:  
          Variables.MyIntVar = 100  
        */  
    }  
  
    public override void Input0_ProcessInputRow(Input0Buffer Row)  
    {  
        /*  
          Add your code here  
        */  
    }  
  
}  
```  
  
#### <a name="additional-project-items-in-the-script-component-project"></a>Autres éléments de projet dans le projet de composant Script  
 Le projet de composant Script peut inclure d’autres éléments que l’élément **ScriptMain** par défaut. Vous pouvez ajouter des classes, des modules, des fichiers de code et des dossiers au projet, et vous pouvez utiliser des dossiers pour organiser des groupes d'éléments.  
  
 Tous les éléments que vous ajoutez sont rendus persistants à l'intérieur du package.  
  
#### <a name="references-in-the-script-component-project"></a>Références dans le projet de composant Script  
 Vous pouvez ajouter des références aux assemblys managés en cliquant avec le bouton droit sur le projet de tâche de script dans l’**Explorateur de projets**, puis en cliquant sur **Ajouter une référence**. Pour plus d’informations, consultez [Référencement d’autres assemblys dans les solutions de script](../../../integration-services/extending-packages-scripting/referencing-other-assemblies-in-scripting-solutions.md).  
  
> [!NOTE]  
>  Vous pouvez consulter des références de projet dans l’environnement de développement intégré VSTA dans l’**Affichage de classes** ou l’**Explorateur de projets**. Vous ouvrez l’une ou l’autre de ces fenêtres à partir du menu **Affichage**. Vous pouvez ajouter une nouvelle référence à partir du menu **Projet**, de l’**Explorateur de projets** ou d’**Affichage de classes**.  
  
## <a name="interacting-with-the-package-in-the-script-component"></a>Interaction avec le package dans le composant Script  
 Le script personnalisé que vous écrivez dans le composant Script peut accéder à des variables et gestionnaires de connexions afin de les utiliser, à partir du package conteneur, via des accesseurs fortement typés dans les classes de base générées automatiquement. Toutefois, vous devez configurer à la fois les variables et les gestionnaires de connexions avant de passer en mode Création du code si vous souhaitez les rendre disponibles pour votre script. Vous pouvez également déclencher des événements et effectuer une journalisation à partir de votre code de composant Script.  
  
 Les éléments de projet générés automatiquement dans le projet de composant Script fournissent les objets, méthodes et propriétés ci-après pour interagir avec le package.  
  
|Fonctionnalité du package|Méthode d'accès|  
|---------------------|-------------------|  
|Variables|Utilisez les propriétés d’accesseur nommées et typées de la classe de collection **Variables** dans l’élément de projet **ComponentWrapper**, exposées via la propriété **Variables** de la classe **ScriptMain**.<br /><br /> La méthode **PreExecute** ne peut accéder qu’à des variables en lecture seule. La méthode **PostExecute** peut accéder à des variables en lecture seule et en lecture/écriture.|  
|Connexions|Utilisez les propriétés d’accesseur typées et nommées de la classe de collection **Connections** dans l’élément de projet **ComponentWrapper**, exposées via la propriété **Connections** de la classe **ScriptMain**.|  
|Événements|Déclenchez des événements à l’aide de la propriété <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent.ComponentMetaData%2A> de la classe **ScriptMain** et des méthodes **Fire\<X>** de l’interface <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100>.|  
|Journalisation|Effectuez la journalisation à l’aide de la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent.Log%2A> de la classe **ScriptMain**.|  
  
## <a name="debugging-the-script-component"></a>Débogage du composant Script  
 Pour déboguer le code dans votre composant Script, définissez au moins un point d'arrêt dans le code, puis fermez l'environnement de développement intégré VISTA pour exécuter le package dans [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)]. Lorsque l'exécution du package entre le composant Script, l'environnement de développement intégré s'ouvre à nouveau et affiche votre code en lecture seule. Lorsque l'exécution atteint le point d'arrêt, vous pouvez examiner les valeurs des variables et exécuter pas à pas le code restant.  
  
> [!NOTE]  
>  Vous ne pouvez pas déboguer un composant Script si vous l'exécutez dans le cadre d'un package enfant exécuté à partir d'une tâche d'exécution de package. Dans ce cas, les points d'arrêt définis dans le composant Script dans le package enfant sont ignorés. Vous pouvez déboguer normalement le package enfant en l'exécutant séparément.  
  
> [!NOTE]  
>  Lorsque vous déboguez un package qui contient plusieurs composants de script, le débogueur n'en débogue qu'un seul. Le système peut déboguer un autre composant script si le débogueur a terminé, comme dans le cas d'une boucle Foreach ou du conteneur de boucles For.  
  
 Vous pouvez également surveiller l'exécution du composant Script en utilisant les méthodes suivantes :  
  
-   Interrompez l’exécution et affichez un message modal en utilisant la méthode **MessageBox.Show** de l’espace de noms **System.Windows.Forms**. (Supprimez ce code une fois que vous avez terminé le processus de débogage.)  
  
-   Déclenchez des événements pour les messages d'information, les avertissements et les erreurs. Les méthodes FireInformation, FireWarning et FireError affichent la description des événements dans la fenêtre **Sortie** de Visual Studio. Toutefois, les méthodes FireProgress, Console.Write et Console.WriteLine n’affichent aucune information dans la fenêtre **Sortie**. Les messages de l’événement FireProgress apparaissent sous l’onglet **Progression** du concepteur [!INCLUDE[ssIS](../../../includes/ssis-md.md)]. Pour plus d’informations, consultez [Déclenchement d’événements dans le composant Script](../../../integration-services/extending-packages-scripting/data-flow-script-component/raising-events-in-the-script-component.md).  
  
-   Journalisez les événements ou les messages définis par l'utilisateur dans les modules fournisseurs d'informations activés. Pour plus d’informations, consultez [Journalisation dans le composant Script](../../../integration-services/extending-packages-scripting/data-flow-script-component/logging-in-the-script-component.md).  
  
 Si vous souhaitez simplement examiner la sortie d’un composant Script configuré en tant que source ou transformation, sans enregistrer les données dans une destination, vous pouvez arrêter le flux de données avec une [transformation Nombre de lignes](../../../integration-services/data-flow/transformations/row-count-transformation.md) et attacher une visionneuse de données à la sortie du composant Script. Pour plus d’informations sur les visionneuses de données, consultez [Débogage d’un flux de données](../../../integration-services/troubleshooting/debugging-data-flow.md).  
  
## <a name="in-this-section"></a>Dans cette section  
 Pour plus d'informations sur le codage du composant Script, consultez les rubriques suivantes de cette section.  
  
 [Présentation du modèle objet du composant Script](../../../integration-services/extending-packages-scripting/data-flow-script-component/understanding-the-script-component-object-model.md)  
 Explique comment utiliser les objets, méthodes et propriétés disponibles dans le composant Script.  
  
 [Référencement d’autres assemblys dans les solutions de script](../../../integration-services/extending-packages-scripting/referencing-other-assemblies-in-scripting-solutions.md)  
 Explique comment référencer des objets de la bibliothèque de classes [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)] dans le composant Script.  
  
 [Simulation d'une sortie d'erreur pour le composant Script](../../../integration-services/extending-packages-scripting-data-flow-script-component-examples/simulating-an-error-output-for-the-script-component.md)  
 Explique comment simuler une sortie d'erreur pour les lignes qui déclenchent des erreurs lors du traitement par le composant Script.  
  
## <a name="external-resources"></a>Ressources externes  
  
-   Entrée de blog, [VSTA setup and configuration troubles for SSIS 2008 and R2 installations](http://go.microsoft.com/fwlink/?LinkId=215661), sur blogs.msdn.com.  
  
## <a name="see-also"></a> Voir aussi  
 [Configuration du composant Script dans l’éditeur de composant de script](../../../integration-services/extending-packages-scripting/data-flow-script-component/configuring-the-script-component-in-the-script-component-editor.md)  
  
  
