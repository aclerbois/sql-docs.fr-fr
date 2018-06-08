---
title: Ajouter ou modifier une expression de propriété | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.component: expressions
ms.reviewer: ''
ms.suite: sql
ms.technology:
- integration-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- expressions [Integration Services], creating
- expressions [Integration Services], property expressions
ms.assetid: cb5da499-065f-4fa6-9f6d-5bc5f385241e
caps.latest.revision: 28
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 423942773422452cc6b46b808fbe53a61270db2f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="add-or-change-a-property-expression"></a>Ajouter ou modifier une expression de propriété
  Vous pouvez créer des expressions de propriété pour les packages, les tâches, les conteneurs de boucles For ou Foreach, les conteneurs de séquences, les gestionnaires d'événements, les gestionnaires de connexions aux niveaux des packages et du projet, ainsi que les modules fournisseurs d'informations.  
  
 Pour créer ou modifier des expressions de propriété, vous pouvez utiliser l’ **Éditeur d’expressions de la propriété** ou le **Générateur d’expressions**. L’ **Éditeur d’expressions de la propriété** est accessible à partir des éditeurs personnalisés disponibles pour les tâches et conteneurs, et à partir de la fenêtre **Propriétés** . Le**Générateur d’expressions** est accessible à partir de l’ **Éditeur d’expressions de la propriété**. Vous pouvez écrire des expressions à la fois dans l’**Éditeur d’expressions de la propriété** et dans le **Générateur d’expressions**, mais le **Générateur d’expressions** fournit un ensemble graphique d’outils qui simplifie la construction d’expressions complexes.  
  
 Pour en savoir plus sur la syntaxe, les opérateurs et les fonctions fournis par [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], consultez [Opérateurs &#40;expression SSIS&#41;](../../integration-services/expressions/operators-ssis-expression.md) et [Fonctions &#40;expression SSIS&#41;](../../integration-services/expressions/functions-ssis-expression.md). La rubrique relative à chaque opérateur ou fonction inclut des exemples d'utilisation de cet opérateur ou de cette fonction dans une expression. Pour obtenir des exemples d’expressions plus complexes, consultez [Expressions de propriété dans des packages](../../integration-services/expressions/use-property-expressions-in-packages.md).  
  
### <a name="to-create-or-change-a-property-expression"></a>Pour créer ou modifier une expression de propriété  
  
1.  Dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], ouvrez le projet contenant le package [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] souhaité.  
  
2.  Dans l'Explorateur de solutions, double-cliquez sur le package pour l'ouvrir, puis effectuez l'une des opérations suivantes :  
  
    -   Si l’élément est une tâche ou un conteneur, double-cliquez dessus, puis cliquez sur **Expressions** dans l’éditeur.  
  
    -   Cliquez avec le bouton droit sur l’élément, puis cliquez sur **Propriétés**.  
  
3.  Cliquez sur la zone **Expressions** , puis sur les points de suspension (...).  
  
4.  Dans l’ **Éditeur d’expressions de la propriété**, sélectionnez une propriété dans la liste **Propriété** , puis effectuez l’une des actions suivantes :  
  
    -   Tapez l’expression de propriété ou modifiez-la directement dans la colonne **Expression** , puis cliquez sur **OK**.  
  
         —ou—  
  
    -   Dans la ligne d’expression de la propriété, cliquez sur les points de suspension (...) pour ouvrir le **Générateur d’expressions**.  
  
5.  (Facultatif) Dans le **Générateur d’expressions**, effectuez l’une des tâches suivantes :  
  
    -   Pour accéder aux variables système et aux variables définies par l’utilisateur, développez **Variables**.  
  
    -   Pour accéder aux fonctions, conversions et opérateurs fournis par le langage d’expression [!INCLUDE[ssIS](../../includes/ssis-md.md)] , développez **Fonctions mathématiques**, **Fonctions de chaîne**, **Fonctions de date et d’heure**, **Fonctions NULL**, **Casts de type**et **Opérateurs**.  
  
    -   Pour générer ou modifier une expression dans le **Générateur d’expressions**, faites glisser les variables, colonnes, fonctions, opérateurs et conversions dans la zone **Expression** ou tapez l’expression dans la zone.  
  
    -   Pour afficher le résultat de l’évaluation de l’expression, cliquez sur **Évaluer l’expression** dans le **Générateur d’expressions**.  
  
         Si l'expression n'est pas valide, une alerte apparaît et décrit les erreurs de syntaxe dans l'expression.  
  
        > [!NOTE]  
        >  L'évaluation d'une expression n'affecte pas le résultat de l'évaluation de la propriété.  
  
6.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
## <a name="see-also"></a> Voir aussi  
 [Expressions Integration Services &#40;SSIS&#41;](../../integration-services/expressions/integration-services-ssis-expressions.md)   
 [Expressions de propriété dans des packages](../../integration-services/expressions/use-property-expressions-in-packages.md)   
 [Packages Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-packages.md)   
 [Conteneurs Integration Services](../../integration-services/control-flow/integration-services-containers.md)   
 [Tâches Integration Services](../../integration-services/control-flow/integration-services-tasks.md)   
 [Gestionnaires d’événements Integration Services &#40;SSIS&#41](../../integration-services/integration-services-ssis-event-handlers.md)   
 [Connexions Integration Services &#40;SSIS&#41;](../../integration-services/connection-manager/integration-services-ssis-connections.md)   
 [Journalisation Integration Services &#40;SSIS&#41;](../../integration-services/performance/integration-services-ssis-logging.md)  
  
  
