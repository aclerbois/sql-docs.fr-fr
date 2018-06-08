---
title: Configuration de la tâche de script dans l’éditeur de tâche de script | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: extending-packages-scripting
ms.reviewer: ''
ms.suite: sql
ms.technology: xml
ms.tgt_pltfrm: ''
ms.topic: reference
applies_to:
- SQL Server 2016 Preview
dev_langs:
- VB
helpviewer_keywords:
- Script task [Integration Services], configuring
- Script Task Editor
- SSIS Script task, configuring
ms.assetid: 232de0c9-b24d-4c38-861d-6c1f4a75bdf3
caps.latest.revision: 35
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 223961d653fb102aa3060341b96f335e216b6e3c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="configuring-the-script-task-in-the-script-task-editor"></a>Configuration de la tâche de script dans l'éditeur de tâche de script
  Avant d’écrire du code personnalisé dans la tâche de script, vous devez configurer ses propriétés principales dans les trois pages de l’**éditeur de tâche de script**. Vous pouvez configurer des propriétés de tâche supplémentaires qui ne sont pas propres à la tâche de script à l'aide de la fenêtre Propriétés.  
  
> [!NOTE]  
>  Contrairement aux versions antérieures pour lesquelles il était possible d'indiquer si les scripts étaient précompilés, tous les scripts sont compilés à partir de [!INCLUDE[ssISversion10](../../../includes/ssisversion10-md.md)].  
  
## <a name="general-page-of-the-script-task-editor"></a>Page Général de l'éditeur de tâche de script  
 La page **Général** de l’**éditeur de tâche de script** vous permet d’assigner un nom unique et une description à la tâche de script.  
  
## <a name="script-page-of-the-script-task-editor"></a>Page Script de l'éditeur de tâche de script  
 La page **Script** de l’**éditeur de tâche de script** affiche les propriétés personnalisées de la tâche de script.  
  
### <a name="scriptlanguage-property"></a>Propriété ScriptLanguage  
 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)] Tools for Applications (VSTA) prend en charge les langages de programmation [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Visual Basic ou [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Visual C#. Après avoir créé un script dans la tâche de script, vous ne pouvez pas modifier la valeur de la propriété **ScriptLanguage**.  
  
 Pour définir le langage de script par défaut des composants Script et des tâches de script, utilisez la propriété **ScriptLanguage** dans la page **Général** de la boîte de dialogue **Options**. Pour plus d'informations, consultez [General Page](https://msdn.microsoft.com/library/ms189436(v=sql.110).aspx).  
  
### <a name="entrypoint-property"></a>Propriété EntryPoint  
 La propriété **EntryPoint** spécifie la méthode de la classe **ScriptMain** du projet VSTA qui est appelée par le runtime [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] comme point d’entrée dans le code de tâche de script. La classe **ScriptMain** est la classe par défaut générée par les modèles de script.  
  
 Si vous modifiez le nom de la méthode dans le projet VSTA, vous devez modifier la valeur de la propriété **EntryPoint** .  
  
### <a name="readonlyvariables-and-readwritevariables-properties"></a>Propriétés ReadOnlyVariables et ReadWriteVariables  
 Vous pouvez entrer des listes délimitées par des virgules de variables existantes comme valeurs de ces propriétés pour rendre les variables accessibles en lecture seule ou en lecture/écriture dans le code de tâche de script. Les deux types de variables sont accessibles dans le code par le biais de la propriété <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.Variables%2A> de l’objet **Dts**. Pour plus d’informations, consultez [Utilisation de variables dans la tâche de script](../../../integration-services/extending-packages-scripting/task/using-variables-in-the-script-task.md).  
  
> [!NOTE]  
>  Les noms de variable respectent la casse.  
  
 Pour sélectionner les variables, cliquez sur le bouton de sélection (**…**) situé en regard du champ de propriété. Pour plus d’informations, consultez [Sélectionner des variables, page](../../../integration-services/control-flow/select-variables-page.md).  
  
### <a name="edit-script-button"></a>Bouton Modifier le script  
 Le bouton **Modifier le script** lance l’environnement de développement VSTA dans lequel vous écrivez votre script personnalisé. Pour plus d’informations, consultez [Codage et débogage de la tâche de script](../../../integration-services/extending-packages-scripting/task/coding-and-debugging-the-script-task.md).  
  
## <a name="expressions-page-of-the-script-task-editor"></a>Page Expressions de l'éditeur de tâche de script  
 La page **Expressions** de l’**éditeur de tâche de script** vous permet d’utiliser des expressions pour fournir les valeurs des propriétés de la tâche de script répertoriées ci-dessus et de nombreuses autres propriétés de tâche. Pour plus d’informations, consultez [Expressions Integration Services &#40;SSIS&#41;](../../../integration-services/expressions/integration-services-ssis-expressions.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Codage et débogage de la tâche de script](../../../integration-services/extending-packages-scripting/task/coding-and-debugging-the-script-task.md)  
  
  
