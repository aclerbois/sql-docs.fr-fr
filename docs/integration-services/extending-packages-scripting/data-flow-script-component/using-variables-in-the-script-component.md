---
title: Utilisation de variables dans le composant Script | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
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
helpviewer_keywords:
- Script component [Integration Services], using variables
ms.assetid: 92d1881a-1ef1-43ae-b1ca-48d0536bdbc2
caps.latest.revision: 13
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 7099b115d3f291117fb40661a513d583f0ee5f89
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="using-variables-in-the-script-component"></a>Utilisation de variables dans le composant Script
  Les variables stockent les valeurs qu'un package et ses conteneurs, tâches et gestionnaires d'événements peuvent utiliser au moment de l'exécution. Pour plus d’informations, consultez [Integration Services &#40;SSIS&#41; Variables](../../../integration-services/integration-services-ssis-variables.md).  
  
 Vous pouvez rendre des variables existantes accessibles en lecture seule ou en lecture/écriture par votre script personnalisé en entrant des listes délimitées par des virgules de variables dans les champs **ReadOnlyVariables** et **ReadWriteVariables** de la page **Script** de l’**Éditeur de transformation de script**. N'oubliez pas que les noms de variables respectent la casse. Utilisez la propriété **Value** pour lire et écrire des variables individuelles. Le composant Script gère tout le verrouillage sous-jacent requis quand votre script manipule les variables au moment de l'exécution.  
  
> [!IMPORTANT]  
>  La collection de **ReadWriteVariables** est uniquement disponible dans la méthode **PostExecute** pour maximiser les performances et réduire le risque de conflits de verrouillage. Par conséquent, vous ne pouvez pas incrémenter directement la valeur d'une variable de package à mesure que vous traitez chaque ligne de données. Incrémentez plutôt la valeur d’une variable locale, puis définissez la valeur de la variable de package sur la valeur de la variable locale dans la méthode **PostExecute** une fois que toutes les données ont été traitées. Vous pouvez également utiliser la propriété <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent.VariableDispenser%2A> pour contourner cette limitation, comme décrit ultérieurement dans cette rubrique. Toutefois, l'écriture directe dans une variable de package au fut et à mesure du traitement de chaque ligne aura des effets négatifs sur les performances et augmentera le risque de conflits de verrouillage.  
  
 Pour plus d’informations sur la page **Scripts** de l’**Éditeur de transformation de script**, consultez [Configuration du composant Script dans l’Éditeur de composant de script](../../../integration-services/extending-packages-scripting/data-flow-script-component/configuring-the-script-component-in-the-script-component-editor.md) et [Éditeur de transformation de script &#40;Page Script&#41;](../../../integration-services/data-flow/transformations/script-transformation-editor-script-page.md).  
  
 Le composant Script crée une classe de collection **Variables** dans l’élément de projet **ComponentWrapper** avec une propriété d’accesseur fortement typée pour la valeur de chaque variable préconfigurée où la propriété porte le même nom que la variable elle-même. Cette collection est exposée via la propriété **Variables** de la classe **ScriptMain**. La propriété de l'accesseur fournit une autorisation de lecture seule ou de lecture/écriture à la valeur de la variable. Par exemple, si vous avez ajouté une variable de type integer nommée `MyIntegerVariable` à la liste **ReadOnlyVariables**, vous pouvez extraire sa valeur dans votre script en utilisant le code suivant :  
  
 `Dim myIntegerVariableValue As Integer = Me.Variables.MyIntegerVariable`  
  
 Vous pouvez également utiliser la propriété <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent.VariableDispenser%2A>, accessible en appelant `Me.VariableDispenser`, pour utiliser des variables dans le composant Script. Dans ce cas, vous n'utilisez pas les propriétés de l'accesseur typé et nommé pour les variables, mais vous accédez directement aux variables. Lorsque vous utilisez la propriété <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent.VariableDispenser%2A>, vous devez gérer à la fois la sémantique de verrouillage et la conversion des types de données pour les valeurs de variables dans votre propre code. Vous devez utiliser la propriété <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent.VariableDispenser%2A> au lieu des propriétés de l'accesseur nommé et typé si vous souhaitez utiliser une variable qui n'est pas disponible au moment du design mais qui est créée par programme au moment de l'exécution.  
  
## <a name="see-also"></a> Voir aussi  
 [Variables Integration Services &#40;SSIS&#41;](../../../integration-services/integration-services-ssis-variables.md)   
 [Utiliser des variables dans des packages](http://msdn.microsoft.com/library/7742e92d-46c5-4cc4-b9a3-45b688ddb787)  
  
  
