---
title: Configuration du composant Script dans l’éditeur de composant de script | Microsoft Docs
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
- SSIS Script component
- Script Component Editor
- SSIS Script component, configuring
- Script component [Integration Services], configuring
ms.assetid: 586dd799-f383-4d6d-b1a1-f09233d14f0a
caps.latest.revision: 46
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 95da332536e968324a61e2001462e4bdae527d24
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="configuring-the-script-component-in-the-script-component-editor"></a>Configuration du composant Script dans l'éditeur de composant de script
  Avant d’écrire du code personnalisé dans le composant Script, vous devez sélectionner le type de composant de flux de données que vous souhaitez créer (source, transformation ou destination), puis configurer les métadonnées et propriétés du composant dans l’**Éditeur de transformation de script**.  
  
## <a name="selecting-the-type-of-component-to-create"></a>Sélection du type de composant à créer  
 Lorsque vous ajoutez un composant Script au volet Flux de données du concepteur [!INCLUDE[ssIS](../../../includes/ssis-md.md)], la boîte de dialogue **Sélectionner le type de composant de script** s’affiche. Préconfigurez le composant en tant que source, transformation ou destination. Une fois cette sélection initiale effectuée, vous pouvez continuer à configurer le composant dans l’**Éditeur de transformation de script**.  
  
 Pour définir le langage de script par défaut du composant Script, utilisez l’option **Langage de script** dans la page **Général** de la boîte de dialogue **Options**. Pour plus d'informations, consultez [General Page](https://msdn.microsoft.com/library/ms189436(v=sql.110).aspx).  
  
## <a name="understanding-the-two-design-time-modes"></a>Fonctionnement des deux modes au moment du design  
 Dans le Concepteur [!INCLUDE[ssIS](../../../includes/ssis-md.md)], le composant Script propose deux modes : le mode Création des métadonnées et le mode Création du code.  
  
 Lorsque vous ouvrez l’**Éditeur de transformation de script**, le composant passe en mode Création des métadonnées. Ce mode vous permet de sélectionner des colonnes d'entrée et d'ajouter ou configurer des sorties et des colonnes de sortie, mais il ne vous permet pas d'écrire du code. Après avoir configuré les métadonnées du composant, vous pouvez basculer en mode Création de code afin d'écrire le script.  
  
 Lorsque vous basculez en mode Création du code en cliquant sur **Modifier le script**, le composant Script verrouille les métadonnées pour empêcher toute modification supplémentaire, puis il génère automatiquement le code de base à partir des métadonnées des entrées et des sorties. Une fois que le code généré automatiquement est complet, vous êtes en mesure d'entrer votre code personnalisé. Votre code utilise les classes de base générées automatiquement pour traiter les lignes d'entrée, accéder à des mémoires tampons et des colonnes dans les mémoires tampons et extraire du package des gestionnaires de connexions et des variables, sous la forme d'objets fortement typés.  
  
 Après avoir entré votre code personnalisé en mode Création de code, vous pouvez revenir en mode Création de métadonnées. Cette opération ne supprime pas le code entré, mais les modifications ultérieures apportées aux métadonnées provoquent la régénération de la classe de base. Après quoi, la validation de votre composant risque d'échouer du fait que des objets référencés par votre code personnalisé peuvent ne plus exister ou avoir été modifiés. Dans ce cas, vous devez corriger votre code manuellement afin qu'il puisse être correctement compilé avec la classe de base régénérée.  
  
## <a name="configuring-the-component-in-metadata-design-mode"></a>Configuration du composant en mode Création de métadonnées  
 Le mode Création de métadonnées vous permet de sélectionner des colonnes d'entrée; et d'ajouter et configurer des sorties et des colonnes de sortie, mais il ne vous permet pas d'écrire du code. Après avoir configuré les métadonnées du composant, basculez en mode Création de code afin d'écrire le script.  
  
 Les propriétés que vous devez configurer dans l'éditeur personnalisé dépendent de l'utilisation du composant Script. Le composant Script peut être configuré en tant que source, transformation ou destination. En fonction de l'utilisation qui est faite du composant, il prendra en charge une entrée ou des sorties, ou bien les deux. Le code personnalisé que vous écrivez traite les lignes et les colonnes d'entrée et de sortie.  
  
### <a name="inputs-columns-page-of-the-script-transformation-editor"></a>Page Colonnes d'entrée de l'Éditeur de transformation de script  
 La page **Colonnes d’entrée** de l’**Éditeur de transformation de script** s’affiche pour les transformations et les destinations, mais pas pour les sources. Cette page vous permet de sélectionner les colonnes d'entrée disponibles à mettre à la disposition de votre script personnalisé et spécifier les accès en lecture seule ou en lecture/écriture à ces colonnes.  
  
 Dans le projet de code qui doit être généré en fonction de ces métadonnées, l'élément de projet BufferWrapper contient une classe pour chaque entrée, et cette classe contient des propriétés d'accesseur typées pour chaque colonne d'entrée sélectionnée. Par exemple, si vous sélectionnez une colonne **CustomerID** de type entier et une colonne **CustomerName** de type chaîne à partir d’une entrée nommée **CustomerInput**, l’élément de projet BufferWrapper contient une classe **CustomerInput** qui dérive de <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptBuffer>. Par ailleurs, la classe **CustomerID** expose une propriété de type entier nommée **CustomerID** et une propriété de type chaîne nommée **CustomerName**. Cette convention permet d'écrire du code avec la vérification de type tel que ci-dessous :  
  
```vb  
Dim currentCustomerID as Integer = CustomerInput.CustomerID  
Dim currentCustomerName as String = CustomerInput.CustomerName  
```  
  
 Pour plus d’informations sur la configuration des colonnes d’entrée d’un type de composant de flux de données spécifique, consultez l’exemple approprié sous [Développement de types spécifiques de composants Script](../../../integration-services/extending-packages-scripting-data-flow-script-component-types/developing-specific-types-of-script-components.md).  
  
### <a name="inputs-and-outputs-page-of-the-script-transformation-editor"></a>Page Entrées et sorties de l'Éditeur de transformation de script  
 La page **Entrées et sorties** de l’**Éditeur de transformation de script** s’affiche pour les sources, les transformations et les destinations. Cette page vous permet d'ajouter, supprimer et configurer des entrées, des sorties et des colonnes de sortie à utiliser dans votre script personnalisé, en tenant compte des contraintes suivantes :  
  
-   Lorsqu'il est utilisé en tant que source, le composant Script ne possède pas d'entrée et prend en charge plusieurs sorties.  
  
-   Lorsqu'il est utilisé en tant que transformation, le composant Script prend en charge une entrée et plusieurs sorties.  
  
-   Lorsqu'il est utilisé en tant que destination, le composant Script prend en charge une entrée et ne possède pas de sortie.  
  
 Dans le projet de code qui doit être généré en fonction de ces métadonnées, l'élément de projet BufferWrapper contient une classe pour chaque entrée et chaque sortie. Par exemple, si vous créez une sortie nommée **CustomerOutput**, l’élément de projet BufferWrapper contient une classe **CustomerOutput** qui dérive de <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptBuffer>, et la classe **CustomerOutput** contient des propriétés d’accesseur typées pour chaque colonne de sortie créée.  
  
 La page **Entrée et sorties** vous permet de configurer uniquement des colonnes de sortie. Vous pouvez sélectionner des colonnes d’entrée pour les transformations et les destinations dans la page **Colonnes d’entrée**. Les propriétés d'accesseur typées créées dans l'élément de projet BufferWrapper sont en écriture seule pour les colonnes de sortie. Les propriétés d’accesseur des colonnes d’entrée peuvent être en lecture seule ou en lecture/écriture selon le type d’utilisation sélectionné pour chaque colonne dans la page **Colonnes d’entrée**.  
  
 Pour plus d’informations sur la configuration des entrées et des sorties d’un type de composant de flux de données spécifique, consultez l’exemple approprié sous [Développement de types spécifiques de composants Script](../../../integration-services/extending-packages-scripting-data-flow-script-component-types/developing-specific-types-of-script-components.md).  
  
> [!NOTE]  
>  Bien qu'il soit impossible de configurer directement une sortie en tant que sortie d'erreur dans le composant Script pour la gestion automatique des lignes d'erreur, vous pouvez reproduire les fonctionnalités d'une sortie d'erreur en créant une sortie supplémentaire et en utilisant un script pour diriger des lignes vers cette sortie, le cas échéant. Pour plus d’informations, consultez [Simulation d’une sortie d’erreur pour le composant Script](../../../integration-services/extending-packages-scripting-data-flow-script-component-examples/simulating-an-error-output-for-the-script-component.md).  
  
#### <a name="exclusiongroup-and-synchronousinputid-properties-of-outputs"></a>Propriétés ExclusionGroup et SynchronousInputID des sorties  
 La propriété **ExclusionGroup** a une valeur non nulle uniquement dans les transformations à sorties synchrones, dans lesquelles votre code effectue des opérations de filtrage ou de création de branche et dirige chaque ligne vers l’une des sorties qui partagent la même valeur **ExclusionGroup** non nulle. Par exemple, la transformation peut diriger des lignes vers la sortie par défaut ou vers une sortie d'erreur. Lorsque vous créez des sorties supplémentaires dans ce scénario, veillez à affecter à la propriété **SynchronousInputID** la valeur de type entier correspondant à l’**ID** de l’entrée du composant.  
  
 La propriété **SynchronousInputID** a une valeur non nulle uniquement dans les transformations à sorties synchrones. Si la valeur de cette propriété est zéro, cela signifie que la sortie est asynchrone. Pour une sortie synchrone, où les lignes sont transférées aux sorties sélectionnées sans que de nouvelles lignes ne soient ajoutées, cette propriété doit contenir l’**ID** de l’entrée du composant.  
  
> [!NOTE]  
>  Lorsque l’**Éditeur de transformation de script** crée la première sortie, l’éditeur attribue à la propriété **SynchronousInputID** de la sortie l’**ID** de l’entrée du composant. Toutefois, lorsque l’éditeur crée les sorties suivantes, il attribue la valeur zéro aux propriétés **SynchronousInputID** de ces sorties.  
>   
>  Si vous créez un composant à sorties synchrones, la propriété **SynchronousInputID** de chaque sortie doit avoir comme valeur l’**ID** de l’entrée du composant. Par conséquent, la valeur zéro de la propriété **SynchronousInputID** de chaque sortie créée par l’éditeur après la première sortie doit être remplacée par l’**ID** de l’entrée du composant.  
>   
>  Si vous créez un composant à sorties asynchrones, la propriété **SynchronousInputID** de chaque sortie doit avoir la valeur zéro. Par conséquent, la valeur de la propriété **SynchronousInputID** de la première sortie, qui correspond à l’**ID** de l’entrée du composant, doit être remplacée par zéro.  
  
 Pour obtenir un exemple de direction de lignes vers l’une des deux sorties synchrones dans le composant Script, consultez [Création d’une transformation synchrone à l’aide du composant Script](../../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-a-synchronous-transformation-with-the-script-component.md).  
  
### <a name="object-names-in-generated-script"></a>Noms d'objets dans le script généré  
 Le composant Script analyse les noms des entrées et des sorties, ainsi que les noms des colonnes dans les entrées et sorties. Puis, en fonction de ces noms, il génère des classes et des propriétés dans l'élément de projet BufferWrapper. Si les noms trouvés contiennent des caractères n’appartenant pas aux catégories Unicode **UppercaseLetter**, **LowercaseLetter**, **TitlecaseLetter**, **ModifierLetter**, **OtherLetter** ou **DecimalDigitLetter**, les caractères non valides sont supprimés des noms générés. Par exemple, les espaces étant supprimés, deux colonnes d’entrée nommées **FirstName** et [**First Name**] sont interprétées comme une seule colonne nommée **FirstName**, ce qui peut entraîner des résultats imprévisibles. Pour éviter cette situation, les noms des entrées et des sorties, ainsi que des colonnes d'entrée et de sortie, utilisés par le composant Script doivent comporter uniquement des caractères appartenant aux catégories Unicode répertoriées dans cette section.  
  
### <a name="script-page-of-the-script-transformation-editor"></a>Page Script de l'Éditeur de transformation de script  
 La page **Script** de l’**Éditeur de tâche de script** vous permet d’assigner un nom unique et une description à la tâche de script. Vous pouvez également assigner des valeurs pour les propriétés suivantes.  
  
> [!NOTE]  
>  Dans [!INCLUDE[ssISversion10](../../../includes/ssisversion10-md.md)] et les versions ultérieures, tous les scripts sont précompilés. Les versions antérieures vous permettaient de spécifier si les scripts étaient précompilés en définissant une propriété **Precompile** pour la tâche.  
  
#### <a name="validateexternalmetadata-property"></a>Propriété ValidateExternalMetadata  
 La valeur booléenne de la propriété **ValidateExternalMetadata** spécifie si le composant doit procéder à une validation par rapport à des sources de données externes au moment de la conception, ou s’il doit reporter la validation jusqu’à moment de l’exécution. Par défaut, cette propriété a la valeur **True** ; c’est-à-dire que les métadonnées externes sont validées au moment de la conception et au moment de l’exécution. Vous pouvez attribuer à cette propriété la valeur **False** lorsqu’une source de données externe n’est pas disponible au moment de la conception : par exemple, lorsque le package télécharge la source ou crée la destination uniquement au moment de l’exécution.  
  
#### <a name="readonlyvariables-and-readwritevariables-properties"></a>Propriétés ReadOnlyVariables et ReadWriteVariables  
 Vous pouvez entrer des listes délimitées par des virgules de variables existantes comme valeurs de ces propriétés pour rendre les variables accessibles en lecture seule ou en lecture/écriture dans le code du composant Script. Les variables sont accessibles dans le code par le biais des propriétés <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent.ReadOnlyVariables%2A> et <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent.ReadWriteVariables%2A> de la classe de base générée automatiquement. Pour plus d’informations, consultez [Utilisation de variables dans le composant Script](../../../integration-services/extending-packages-scripting/data-flow-script-component/using-variables-in-the-script-component.md).  
  
> [!NOTE]  
>  Les noms de variable respectent la casse.  
  
#### <a name="scriptlanguage"></a>ScriptLanguage  
 Vous pouvez sélectionner [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Visual Basic ou [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Visual C# comme langage de programmation du composant Script.  
  
#### <a name="edit-script-button"></a>Bouton Modifier le script  
 Le bouton **Modifier le script** permet d’ouvrir l’environnement de développement intégré [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)] Tools for Applications (VSTA) dans lequel vous écrivez votre script personnalisé. Pour plus d’informations, consultez [Codage et débogage du composant Script](../../../integration-services/extending-packages-scripting/data-flow-script-component/coding-and-debugging-the-script-component.md).  
  
### <a name="connection-managers-page-of-the-script-transformation-editor"></a>Page Gestionnaires de connexions de l'Éditeur de transformation de script  
 La page **Gestionnaires de connexions** de l’**Éditeur de transformation de script** vous permet d’ajouter et de supprimer des gestionnaires de connexions que vous souhaitez utiliser dans votre script personnalisé. En principe, vous devez référencer des gestionnaires de connexions lorsque vous créez un composant source ou un composant de destination.  
  
 Dans le projet de code qui doit être généré selon ces métadonnées, l’élément de projet **ComponentWrapper** contient une classe de collection **Connections** qui possède une propriété d’accesseur typée pour chaque gestionnaire de connexions sélectionné. Chaque propriété d'accesseur typée porte le même nom que le gestionnaire de connexions lui-même et retourne une référence à ce dernier sous la forme d'une instance de <xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.IDTSConnectionManager100>. Par exemple, si vous avez ajouté un gestionnaire de connexions nommé `MyADONETConnection` dans la page **Gestionnaires de connexions** de l’éditeur, vous pouvez obtenir une référence au gestionnaire de connexions dans votre script à l’aide du code suivant :  
  
```vb  
Dim myADONETConnectionManager As IDTSConnectionManager100 = _  
    Me.Connections.MyADONETConnection  
```  
  
 Pour plus d’informations, consultez [Connexion aux sources de données dans le composant Script](../../../integration-services/extending-packages-scripting/data-flow-script-component/connecting-to-data-sources-in-the-script-component.md).  
  
## <a name="see-also"></a> Voir aussi  
 [Codage et débogage du composant Script](../../../integration-services/extending-packages-scripting/data-flow-script-component/coding-and-debugging-the-script-component.md)  
  
  
