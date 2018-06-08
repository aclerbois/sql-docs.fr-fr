---
title: Variables Integration Services (SSIS) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: non-specific
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- variables [Integration Services], passing between packages
- user-defined variables [Integration Services]
- scope [Integration Services]
- system variables [Integration Services]
- variables [Integration Services]
- variables [Integration Services], about variables
- values [Integration Services]
ms.assetid: c1e81ad6-628b-46d4-9b09-d2866517b6ca
caps.latest.revision: 60
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 40acc806d038e85c206d0861bb28575085483ade
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="integration-services-ssis-variables"></a>Variables Integration Services (SSIS)
  Les variables stockent des valeurs qu'un package [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] et ses conteneurs, tâches et gestionnaires d'événements peuvent utiliser au moment de l'exécution. Les scripts de la tâche de script et du composant Script peuvent également utiliser des variables. Les contraintes de précédence qui séquencent les tâches et les conteneurs dans un flux de travail peuvent utiliser des variables lorsque leurs définitions de contraintes incluent des expressions.  
  
 Vous pouvez utiliser des variables dans des packages [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] pour les opérations suivantes :  
  
-   Mise à jour de propriétés d'éléments de package au moment de l'exécution. Par exemple, vous pouvez dynamiquement définir le nombre d'exécutables simultanés qu'un conteneur de boucles Foreach autorise.  
  
-   Inclusion d'une table de recherche en mémoire. Par exemple, un package peut exécuter une tâche d'exécution SQL qui charge une variable avec des valeurs de données.  
  
-   Chargement de variables avec des valeurs de données, puis utilisation de ces variables pour spécifier une condition de recherche dans une clause WHERE. Par exemple, le script dans une tâche de script peut mettre à jour la valeur d'une variable qui est utilisée par une instruction Transact-SQL dans une tâche d'exécution SQL.  
  
-   Chargement d'une variable avec un entier, puis utilisation de la valeur pour contrôler le bouclage au sein d'un flux de contrôle de package. Par exemple, vous pouvez utiliser une variable dans l'expression d'évaluation d'un conteneur de boucles For pour contrôler l'itération.  
  
-   Remplissage de valeurs de paramètres pour des instructions Transact-SQL au moment de l'exécution. Par exemple, un package peut exécuter une tâche d'exécution SQL, puis utiliser des variables pour définir dynamiquement les paramètres d'une instruction Transact-SQL.  
  
-   Création d'expressions qui incluent des valeurs de variables. Par exemple, la transformation de colonnes dérivées peut remplir une colonne avec le résultat obtenu suite à la multiplication d'une valeur de variable par une valeur de colonne.  
  
## <a name="system-and-user-defined-variables"></a>Variables système et variables définies par l'utilisateur  
 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] prend en charge deux types de variables : les variables définies par l’utilisateur et les variables système. Les variables définies par l'utilisateur sont définies par les développeurs de packages, tandis que les variables système sont définies par [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)]. Vous pouvez créer autant de variables définies par l'utilisateur qu'un package l'exige, mais vous ne pouvez pas créer de variables système supplémentaires.  
  
 Toutes les variables (système et définies par l'utilisateur) peuvent être utilisées dans les liaisons de paramètres que la tâche d'exécution SQL utilise pour mapper des variables à des paramètres dans des instructions SQL. Pour plus d’informations, consultez [Tache d’exécution de requêtes SQL](../integration-services/control-flow/execute-sql-task.md) et [Paramètres et codes de retour dans la tâche d’exécution SQL](http://msdn.microsoft.com/library/a3ca65e8-65cf-4272-9a81-765a706b8663).  
  
> [!NOTE]  
>  Les noms des variables définies par l’utilisateur et des variables système respectent la casse.  
  
 Vous pouvez créer des variables définies par l’utilisateur pour tous les types de conteneurs [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] : packages, conteneurs de boucles Foreach, conteneurs de boucles For, conteneurs de séquences, tâches et gestionnaires d’événements. Les variables définies par l'utilisateur sont des membres de la collection Variables du conteneur.  
  
 Si vous créez le package à l'aide du concepteur [!INCLUDE[ssIS](../includes/ssis-md.md)] , vous pouvez afficher les membres de la collection Variables dans le dossier **Variables** sous l'onglet **Explorateur de package** du concepteur [!INCLUDE[ssIS](../includes/ssis-md.md)] . Les dossiers répertorient les variables définies par l'utilisateur et les variables système.  
  
 Vous pouvez configurer les variables définies par l'utilisateur de plusieurs manières :  
  
-   Spécifiez un nom et une description pour la variable.  
  
-   Spécifiez un espace de noms pour la variable.  
  
-   Indiquez si la variable déclenche un événement lorsque sa valeur change.  
  
-   Indiquez si la variable est en lecture seule ou accessible en lecture/écriture.  
  
-   Utilisez le résultat de l'évaluation d'une expression pour définir la valeur de la variable.  
  
-   Créez la variable dans la portée du package ou un objet de package tel qu'une tâche.  
  
-   Spécifiez la valeur et le type de données de la variable.  
  
 La seule option configurable des variables système est le déclenchement d'un événement lorsque leur variable change.  
  
 Un ensemble de variables système différent est disponible pour différents types de conteneurs. Pour plus d'informations sur les variables système utilisées par les packages et leurs éléments, consultez [System Variables](../integration-services/system-variables.md).  
  
 Pour plus d’informations sur les scénarios d’utilisation concrète des variables, consultez [Utiliser des variables dans des packages](http://msdn.microsoft.com/library/7742e92d-46c5-4cc4-b9a3-45b688ddb787).  
  
## <a name="properties-of-variables"></a>Propriétés des variables  
 Vous pouvez configurer les variables définies par l’utilisateur en définissant les propriétés suivantes dans la fenêtre **Variables** ou la fenêtre **Propriétés** . Certaines propriétés sont disponibles uniquement dans la fenêtre Propriétés.  
  
> [!NOTE]  
>  La seule option configurable des variables système est le déclenchement d'un événement lorsque leur variable change.  
  
 **Description**    
 Spécifie la description de la variable.  
  
 **EvaluateAsExpression**    
 Quand la propriété a la valeur **True**, l’expression fournie est utilisée pour définir la valeur de la variable.  
  
 **Expression**    
 Spécifie l'expression affectée à la variable.  
  
 **Nom**    
 Spécifie le nom de la variable.  
  
 **Espace de noms**  
 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] fournit deux espaces de noms, **User** et **System**. Par défaut, les variables personnalisées se trouvent dans l’espace de noms **User** et les variables système se trouvent dans l’espace de noms **System** . Vous pouvez créer des espaces de noms supplémentaires pour les variables définies par l’utilisateur et changer le nom de l’espace de noms **User** , mais vous ne pouvez pas modifier le nom de l’espace de noms **System** , ajouter des variables à l’espace de noms **System** , ni affecter des variables système à un autre espace de noms.  
  
**RaiseChangedEvent**  
 Quand la propriété a la valeur **True**, l’événement **OnVariableValueChanged** est déclenché en cas de changement de la valeur de la variable.  
  
 **En lecture seule**  
 Quand la propriété a la valeur **False**, la variable est en lecture-écriture.  
  
**Portée**    
 > [!NOTE]  
>  Vous pouvez modifier ce paramètre de propriété uniquement en cliquant sur **Déplacer la variable** dans la fenêtre **Variables** .  
  
 Une variable est créée dans la portée d'un package ou dans la portée d'un conteneur, d'une tâche ou d'un gestionnaire d'événements dans le package. Le conteneur de packages se trouvant au sommet de la hiérarchie de conteneurs, les variables avec une portée de package fonctionnent comme les variables globales et peuvent être utilisées par tous les conteneurs du package. De même, les variables définies dans la portée d'un conteneur tel qu'un conteneur de boucles For peuvent être utilisées par toutes les tâches ou les conteneurs situés dans le conteneur de boucles For.  
  
 Si un package exécute d'autres packages par le biais de la tâche d'exécution de package, les variables définies dans la portée du package appelant ou de la tâche d'exécution de package peuvent être mises à disposition du package appelé à l'aide du type de configuration Variable de package parent. Pour plus d’informations, consultez [Package Configurations](../integration-services/packages/package-configurations.md).  
  
**IncludeInDebugDump**  
 Indiquez si la valeur variable est incluse dans les fichiers de vidage de débogage.  
  
 Pour les variables définies par l’utilisateur et les variables système, la valeur par défaut de l’option **InclueInDebugDump** est **true**.  
  
 Toutefois, pour les variables définies par l’utilisateur, le système réinitialise l’option **IncludeInDebugDump** avec la valeur **false** quand les conditions suivantes sont remplies :  
  
-   Si la propriété de variable **valuateAsExpression** a la valeur **true**, le système réinitialise l'option **IncludeInDebugDump** à **false**.  
  
     Pour inclure le texte de l'expression en tant que valeur variable dans les fichiers de vidage de débogage, affectez la valeur **true** à l'option **IncludeInDebugDump**.  
  
-   Si le type de données de variable est changé en une chaîne, le système réinitialise l'option **IncludeInDebugDump** à **false**.  
  
 Lorsque le système réinitialise l'option **IncludeInDebugDump** à **false**, cela peut remplacer la valeur sélectionnée par l'utilisateur.  
  
**Value**    
La valeur d'une variable définie par l'utilisateur peut être un littéral ou une expression. La valeur d’une variable ne peut pas être Null. Les variables ont les valeurs par défaut suivantes :

| Type de données | Valeur par défaut |
|---|---|
| Booléen | False |
| Types de données numérique et binaire | 0 (zéro) |
| Types de données de caractère et de chaîne | (chaîne vide) |
| Object | System.Object |
| | |

Une variable a des options permettant de définir la valeur de la variable et le type de données de la valeur. Les deux propriétés doivent être compatibles : par exemple, l'utilisation d'une valeur de chaîne avec un type de données Integer n'est pas valide.  
  
 Si la variable est configurée de façon à correspondre à une expression, vous devez fournir une expression. Au moment de l'exécution, l'expression est évaluée et le résultat de l'évaluation est affecté comme valeur de la variable. Par exemple, si une variable utilise l'expression `DATEPART("month", GETDATE())` , la valeur de la variable est l'équivalent numérique du mois de la date actuelle. L'expression doit être une expression valide qui utilise la syntaxe de grammaire d'expression [!INCLUDE[ssIS](../includes/ssis-md.md)] . Lorsqu'une expression est utilisée avec des variables, elle peut utiliser des littéraux et les opérateurs et fonctions fournis par la grammaire d'expression, mais elle ne peut pas faire référence aux colonnes d'un flux de données du package. La longueur maximale d'une expression est limitée à 4 000 caractères. Pour plus d’informations, consultez [Expressions Integration Services &#40;SSIS&#41;](../integration-services/expressions/integration-services-ssis-expressions.md).  
  
**ValueType**    
 > [!NOTE]  
>  La valeur de la propriété apparaît dans la colonne **Type de données** de la fenêtre **Variables** .  
  
 Spécifie le type de données de la valeur de la variable.  

## <a name="scenarios-for-using-variables"></a>Scénarios d’utilisation des variables  
 Les variables sont utilisées de nombreuses manières différentes dans les packages [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] . Vous constaterez sans doute que le développement de package est limité tant que vous n'avez pas ajouté une variable définie par l'utilisateur à votre package pour mettre en œuvre la flexibilité et la gestion nécessaires à votre solution. En fonction du scénario, les variables système sont aussi couramment utilisées.  
  
 **Expressions de propriété** : les variables permettent de fournir des valeurs dans les expressions de propriété qui définissent les propriétés d’objets de package et de packages. Par exemple, l’expression `SELECT * FROM @varTableName` inclut la variable `varTableName` qui met à jour l’instruction SQL exécutée par une tâche d’exécution SQL. L’expression `DATEPART("d", GETDATE()) == 1? @[User::varPackageFirst]:@[User::varPackageOther]`met à jour le package exécuté par la tâche d’exécution de package en exécutant le package spécifié dans la variable `varPackageFirst` le premier jour du mois et en exécutant le package spécifié dans la variable `varPackageOther` les autres jours. Pour plus d’informations, consultez [Expressions de propriété dans des packages](../integration-services/expressions/use-property-expressions-in-packages.md).  
  
 **Expressions de flux de données** : les variables permettent de fournir des valeurs dans les expressions dont se servent les transformations de fractionnement conditionnel et de colonne dérivée pour remplir les colonnes ou diriger les lignes de données vers différentes sorties de transformation. Par exemple, l'expression `@varSalutation + LastName`concatène la valeur dans la variable `VarSalutation` et la colonne `LastName` . L’expression `Income < @HighIncome` dirige vers une sortie les lignes de données dans lesquelles la valeur de la colonne `Income` est inférieure à la valeur de la variable `HighIncome`. Pour plus d’informations, consultez [Transformation de colonne dérivée](../integration-services/data-flow/transformations/derived-column-transformation.md), [Transformation de fractionnement conditionnel](../integration-services/data-flow/transformations/conditional-split-transformation.md) et [Expressions Integration Services &#40;SSIS&#41;](../integration-services/expressions/integration-services-ssis-expressions.md).  
  
 **Expressions de contrainte de précédence** : elles fournissent les valeurs à utiliser dans des contraintes de précédence pour déterminer si un exécutable contraint s’exécute. Les expressions peuvent être utilisées avec un résultat d'exécution (succès, échec, achèvement de l'opération) ou à la place d'un résultat d'exécution. Par exemple, si l’expression `@varMax > @varMin`est évalué à **true**, l’exécutable s’exécute. Pour plus d’informations, consultez [Ajouter des expressions aux contraintes de précédence](http://msdn.microsoft.com/library/5574d89a-a68e-4b84-80ea-da93305e5ca1).  
  
 **Paramètres et codes de retour** : ils fournissent des valeurs aux paramètres d’entrée ou stockent les valeurs des paramètres de sortie et des codes de retour. Cette opération s'effectue en mappant les variables aux paramètres et aux valeurs de retour. Par exemple, si vous affectez à la variable `varProductId` la valeur 23 et que vous exécutez l’instruction SQL `SELECT * from Production.Product WHERE ProductID = ?`, la requête récupère le produit associé à la valeur `ProductID` 23. Pour plus d’informations, consultez [Tache d’exécution de requêtes SQL](../integration-services/control-flow/execute-sql-task.md) et [Paramètres et codes de retour dans la tâche d’exécution SQL](http://msdn.microsoft.com/library/a3ca65e8-65cf-4272-9a81-765a706b8663).  
  
 **Expressions de boucle For** : elles fournissent les valeurs à utiliser dans les expressions d’initialisation, d’évaluation et d’assignation de la boucle For. Par exemple, si la variable `varCount` a la valeur 2 et la variable `varMaxCount` la valeur 10 et que l’expression d’initialisation est `@varCount`, l’expression d’évaluation  `@varCount < @varMaxCount`et l’expression d’assignation `@varCount =@varCount +1`, la boucle se répète 8 fois. Pour plus d’informations, consultez [Conteneur de boucles For](../integration-services/control-flow/for-loop-container.md).  
  
 **Configurations de variable de package parent** : elles permettent de passer des valeurs des packages parents aux packages enfants. Les packages enfants peuvent accéder à des variables dans le package parent à l'aide de configurations de variable de package parent. Par exemple, si le package enfant doit utiliser la même date que le package parent, le package enfant peut définir une configuration de variable de package parent qui spécifie une variable définie par la fonction GETDATE dans le package parent. Pour plus d’informations, consultez [Tâche d’exécution de package](../integration-services/control-flow/execute-package-task.md) et [Configurations de package](../integration-services/packages/package-configurations.md).  
  
 **Tâche de script et composant de script** : ils fournissent une liste de variables en lecture seule et en lecture/écriture à la tâche de script ou au composant de script, mettent à jour les variables en lecture/écriture dans le script, puis utilisent les valeurs mises à jour à l’intérieur ou à l’extérieur du script. Par exemple, dans le code `numberOfCars = CType(Dts.Variables("NumberOfCars").Value, Integer)`, la variable de script `numberOfCars` est mise à jour par la valeur dans la variable `NumberOfCars`. Pour plus d’informations, consultez [Utilisation de variables dans la tâche de script](../integration-services/extending-packages-scripting/task/using-variables-in-the-script-task.md).  

## <a name="add-a-variable"></a>Ajouter une variable  
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)], ouvrez le package [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] que vous voulez utiliser.  
  
2.  Dans l'Explorateur de solutions, double-cliquez sur le package pour l'ouvrir.  
  
3.  Dans le concepteur [!INCLUDE[ssIS](../includes/ssis-md.md)] , pour définir la portée de la variable, effectuez l'une des opérations suivantes :  
  
    -   Pour définir la portée du package, cliquez n’importe où sur l’aire de conception de l’onglet **Flux de contrôle** .  
  
    -   Pour définir la portée d’un gestionnaire d’événements, sélectionnez un exécutable et un gestionnaire d’événements sur l’aire de conception de l’onglet **Gestionnaire d’événements** .  
  
    -   Pour définir la portée d’une tâche ou d’un conteneur, cliquez sur une tâche ou un conteneur sur l’aire de conception de l’onglet **Flux de contrôle** ou de l’onglet **Gestionnaire d’événements** .  
  
4.  Dans le menu **SSIS** , cliquez sur **Variables**. Vous pouvez éventuellement afficher la fenêtre **Variables** en mappant la commande View.Variables avec une combinaison de clés de votre choix dans la page **Clavier** de la boîte de dialogue **Options** .  
  
5.  Dans la fenêtre **Variables** , cliquez sur l’icône **Ajouter une variable** . La nouvelle variable est ajoutée à la liste.  
  
6.  Dans la boîte de dialogue **Options de la grille** , sélectionnez des colonnes supplémentaires à afficher dans la boîte de dialogue **Options de la grille de variables** , puis cliquez sur **OK**.  
  
7.  Éventuellement, définissez les propriétés d'une variable. Pour plus d’informations, consultez [Définir les propriétés d’une variable définie par l’utilisateur](http://msdn.microsoft.com/library/f98ddbec-f668-4dba-a768-44ac3ae0536f).  
  
8.  Pour enregistrer le package mis à jour, cliquez sur **Enregistrer les éléments sélectionnés** dans le menu **Fichier** .  

### <a name="add-variable-dialog-box"></a>Ajouter une variable, boîte de dialogue
Utilisez la boîte de dialogue **Ajouter une variable** pour spécifier les propriétés d'une nouvelle variable.  
  
#### <a name="options"></a>Options  
 **Conteneur**  
 Sélectionnez un conteneur dans la liste. Le conteneur définit l'étendue de la variable. Le conteneur peut être le package ou un exécutable du package.  
  
 **Nom**  
 Entrez le nom de la variable.  
  
 **Espace de noms**  
 Spécifiez l'espace de noms de la variable. Par défaut, les variables définies par l’utilisateur sont dans l’espace de noms **Utilisateur** .  
  
 **Type de valeur**  
 Sélectionnez un type de données.  
  
 **Value**  
 Tapez une valeur. La valeur doit être compatible avec le type de données spécifié dans l'option **Type de valeur** .  
  
 **Lecture seule**  
 Sélectionnez cette option pour que la variable soit en lecture seule.  
   
## <a name="delete-a-variable"></a>Supprimer une variable  
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)], ouvrez le projet [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] contenant le package souhaité.  
  
2.  Dans l'Explorateur de solutions, cliquez avec le bouton droit sur le package pour l'ouvrir.  
  
3.  Dans le menu **SSIS** , cliquez sur **Variables**. Vous pouvez éventuellement afficher la fenêtre **Variables** en mappant la commande View.Variables avec une combinaison de clés de votre choix dans la page **Clavier** de la boîte de dialogue **Options** .  
  
4.  Sélectionnez la variable à supprimer, puis cliquez sur **Supprimer la variable**.  
  
     Si vous ne voyez pas la variable dans la fenêtre variables, cliquez sur **Options de la grille** puis sélectionnez **Afficher les variables de toutes les étendues**.  
  
5.  Si la boîte de dialogue **Confirmer la suppression des variables** apparaît, cliquez sur **Oui** pour confirmer.  
  
6.  Pour enregistrer le package mis à jour, cliquez sur **Enregistrer les éléments sélectionnés** dans le menu **Fichier** .  
  
## <a name="change-the-scope-of-a-variable"></a>Modifier l’étendue d’une variable  
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)], ouvrez le projet [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] contenant le package souhaité.  
  
2.  Dans l'Explorateur de solutions, cliquez avec le bouton droit sur le package pour l'ouvrir.  
  
3.  Dans le menu **SSIS** , cliquez sur **Variables**. Vous pouvez éventuellement afficher la fenêtre **Variables** en mappant la commande View.Variables avec une combinaison de clés de votre choix dans la page **Clavier** de la boîte de dialogue **Options** .  
  
4.  Sélectionnez la variable, puis cliquez sur **Déplacer la variable**.  
  
     Si vous ne voyez pas la variable dans la fenêtre variables, cliquez sur **Options de la grille** puis sélectionnez **Afficher les variables de toutes les étendues**.  
  
5.  Dans la boîte de dialogue **Sélectionner une nouvelle étendue** , sélectionnez le package ou un conteneur, une tâche ou un gestionnaire d'événements dans le package pour modifier l'étendue de la variable.  
  
6.  Pour enregistrer le package mis à jour, cliquez sur **Enregistrer les éléments sélectionnés** dans le menu **Fichier** .  

## <a name="set-the-properties-of-a-user-defined-variable"></a>Définir les propriétés d'une variable définie par l'utilisateur
 Pour définir les propriétés d'une variable définie par l'utilisateur dans [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)], vous pouvez utiliser l'une des fonctionnalités suivantes :  
  
-   Fenêtre Variables.  
  
-   Fenêtre Propriétés. La fenêtre **Propriétés** répertorie les propriétés pour la configuration des variables qui ne sont pas disponibles dans la fenêtre **Variables** : Description, EvaluateAsExpression, Expression, ReadOnly, ValueType et IncludeInDebugDump.  
  
> [!NOTE]  
>  [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] fournit également un ensemble de variables système dont les propriétés ne peuvent pas être mises à jour, à l’exception de la propriété RaiseChangedEvent.  
  
### <a name="set-expressions-on-variables"></a>Définir des expressions dans des variables  
  
 Quand vous utilisez la fenêtre **Propriétés** pour définir des expressions sur une variable définie par l’utilisateur :  
  
-   La valeur d’une variable peut être définie par la propriété Valeur ou la propriété Expression. Par défaut, la propriété EvaluateAsExpression a la valeur **False** et la valeur de la variable est définie par la propriété Valeur. Pour utiliser une expression pour définir la valeur, vous devez commencer par définir EvaluateAsExpression sur **True**, puis fournir une expression dans la propriété Expression. Le résultat de l’évaluation de l’expression est automatiquement affecté à la propriété Valeur.  
  
-   La propriété ValueType contient le type de données de la valeur de la propriété Valeur. Quand Valeur est définie par une expression, ValueType est automatiquement mis à jour avec un type de données compatible avec le résultat de l’évaluation de l’expression. Par exemple, si Valeur contient 0 et la propriété ValueType contient **Int32** , et que vous définissez ensuite Expression à GETDATE(), Valeur contient la date et l’heure et ValueType est défini sur **DateTime**.  
  
-   La fenêtre **Propriétés** pour la variable permet d’accéder à la boîte de dialogue **Générateur d’expressions** . Vous pouvez utiliser cet outil pour créer, valider et évaluer des expressions. Pour plus d’informations, consultez [Générateur d’expressions](../integration-services/expressions/expression-builder.md) et [Expressions Integration Services &#40;SSIS&#41;](../integration-services/expressions/integration-services-ssis-expressions.md).  
  
 Quand vous utilisez la fenêtre **Variables** pour définir des expressions sur une variable définie par l’utilisateur :  
  
-   Pour utiliser une expression pour définir la valeur de la variable, vérifiez d’abord que le type de variable est compatible avec le résultat de l’évaluation de l’expression et fournissez ensuite une expression dans la colonne **Expression** de la fenêtre **Variables** . La propriété EvaluateAsExpression dans la fenêtre **Propriétés** est définie automatiquement sur **True**.  
  
-   Lorsque vous affectez une expression à une variable, un marqueur spécial sous la forme d'une icône s'affiche en regard de la variable. Ce marqueur d'icône spécial s'affiche également en regard des gestionnaires de connexions et des tâches contenant des expressions.  
  
-   La fenêtre **Variables** pour la variable permet d’accéder à la boîte de dialogue **Générateur d’expressions** . Vous pouvez utiliser cet outil pour créer, valider et évaluer des expressions. Pour plus d’informations, consultez [Générateur d’expressions](../integration-services/expressions/expression-builder.md) et [Expressions Integration Services &#40;SSIS&#41;](../integration-services/expressions/integration-services-ssis-expressions.md).  
  
 Dans les fenêtres **Variables** et **Propriétés**, si vous affectez une expression à la variable et que **EvaluateAsExpression** est défini sur **True**, vous ne pouvez pas changer le type de données de la variable.  
  
### <a name="set-the-namespace-and-name-properties"></a>Définir les propriétés Espace de noms et Nom
  
 La valeur des propriétés **Nom** et **Espace de noms** doit commencer par une lettre de l’alphabet, conformément à la convention Unicode Standard 2.0, ou par un trait de soulignement (_). Les caractères suivants peuvent être des lettres ou des chiffres, conformément à la convention Unicode standard 2.0, ou un trait de soulignement (\_).  
  
### <a name="set-variable-properties-in-the-variables-window"></a>Définir les propriétés de la variable dans la fenêtre Variables   
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)], ouvrez le projet [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] contenant le package souhaité.  
  
2.  Dans l'Explorateur de solutions, cliquez avec le bouton droit sur le package pour l'ouvrir.  
  
3.  Dans le menu **SSIS** , cliquez sur **Variables**.  
  
     Vous pouvez éventuellement afficher la fenêtre **Variables** en mappant la commande View.Variables avec une combinaison de clés de votre choix dans la page **Clavier** de la boîte de dialogue **Options** .  
  
4.  Éventuellement, dans la fenêtre **Variables** cliquez sur **Options de la grille**, puis sélectionnez les colonnes qui apparaissent dans la fenêtre **Variables** et sélectionnez les filtres à appliquer à la liste des variables.  
  
5.  Sélectionnez la variable dans la liste, puis mettez à jour les valeurs dans les colonnes **Nom**, **Type de données**, **Valeur**, **Espace de noms**, **Déclencher un événement de modification**, **Description** et **Expression** .  
  
6.  Sélectionnez la variable dans la liste, puis cliquez sur **Déplacer la variable** pour modifier l’étendue.  
  
7.  Pour enregistrer le package mis à jour, dans le menu **Fichier** , cliquez sur **Enregistrer les éléments sélectionnés**.  
  
### <a name="set-variable-properties-in-the-properties-window"></a>Définir les propriétés de la variable dans la fenêtre Propriétés  

1.  Dans [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)], ouvrez le projet [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] contenant le package souhaité.  
  
2.  Dans l'Explorateur de solutions, cliquez avec le bouton droit sur le package pour l'ouvrir.  
  
3.  Dans le menu **Affichage** , cliquez sur **Fenêtre Propriétés**.  
  
4.  Dans le Concepteur [!INCLUDE[ssIS](../includes/ssis-md.md)] , cliquez sur l’onglet **Explorateur de package** , puis développez le nœud Package.  
  
5.  Pour modifier les variables avec une portée de package, développez le nœud Variables. Sinon, développez les nœuds Gestionnaires d'événements ou Exécutables jusqu'à ce que vous trouviez le nœud Variables contenant la variable que vous voulez modifier.  
  
6.  Cliquez sur la variable dont vous souhaitez modifier les propriétés.  
  
7.  Dans la fenêtre **Propriétés** , mettez à jour les propriétés en lecture/écriture de la variable. Certaines propriétés sont en lecture/lecture uniquement pour les variables définies par l'utilisateur.  
  
     Pour plus d’informations sur les propriétés, consultez [Variables Integration Services &#40;SSIS&#41;](../integration-services/integration-services-ssis-variables.md).  
  
8.  Pour enregistrer le package mis à jour, dans le menu **Fichier** , cliquez sur **Enregistrer les éléments sélectionnés**.  

## <a name="update-a-variable-dynamically-with-configurations"></a>Mettre à jour une variable de manière dynamique avec des configurations  
 Pour mettre à jour dynamiquement des variables, vous pouvez créer des configurations pour les variables, déployer les configurations dans le package, puis mettre à jour les valeurs de variable dans un fichier de configuration lorsque vous déployez les packages. À l'exécution, le package utilise les valeurs de variable mises à jour. Pour plus d’informations, consultez [Créer des configurations de package](../integration-services/packages/create-package-configurations.md).  

## <a name="related-tasks"></a>Related Tasks  
 [Utiliser les valeurs des variables et des paramètres dans un package enfant](../integration-services/packages/legacy-package-deployment-ssis.md#child)  
  
 [Mapper des paramètres de requête à des variables dans un composant de flux de données](../integration-services/data-flow/map-query-parameters-to-variables-in-a-data-flow-component.md)  
